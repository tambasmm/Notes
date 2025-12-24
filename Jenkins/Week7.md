
***

## 1) Minimal Declarative Pipeline

```groovy
pipeline {
  agent any          // where to run

  stages {
    stage('Build') {
      steps {
        echo 'Building...'
        sh 'make build'
      }
    }
  }
}
```

***

## 2) Canonical Structure (Cheat Sheet)

```groovy
pipeline {
  agent any | none | { label 'linux' | docker { image 'node:20' args '-u root' } }

  environment {
    APP_ENV = 'dev'
    // Credentials example:
    // SECRET = credentials('my-secret-id')
  }

  options {
    timeout(time: 20, unit: 'MINUTES')
    buildDiscarder(logRotator(numToKeepStr: '20'))
    skipDefaultCheckout()          // if you handle checkout yourself
    disableConcurrentBuilds()
    ansiColor('xterm')
    // quietPeriod(0), timestamps(), etc.
  }

  parameters {
    string(name: 'BRANCH', defaultValue: 'main', description: 'Git branch')
    booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests?')
    choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Target env')
  }

  tools {
    jdk 'temurin-17'
    maven 'maven-3.9.6'
    // nodejs 'node-20'
  }

  triggers {
    cron('H/15 * * * *')          // every ~15 minutes
    // pollSCM('H/2 * * * *')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      when {
        expression { params.ENV != 'prod' }  // conditional stage
      }
      steps {
        sh 'mvn -B -U -DskipTests package'
      }
      post {
        success { echo 'Build artifacts created.' }
        failure { echo 'Build failed.' }
      }
    }

    stage('Test') {
      when { equals expected: true, actual: params.RUN_TESTS }
      options { retry(2) }
      steps {
        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
          sh 'mvn test'
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Parallel Quality Gates') {
      parallel {
        stage('Lint') {
          steps { sh 'npm run lint' }
        }
        stage('SAST') {
          steps { sh './scan.sh' }
        }
      }
    }

    stage('Deploy') {
      agent { label 'deploy-nodes' }
      when { branch 'main' }
      steps {
        timeout(time: 10, unit: 'MINUTES') {
          input message: 'Deploy to PROD?', ok: 'Deploy'
        }
        sh './deploy.sh'
      }
    }
  }

  post {
    always {
      echo "Final status (read-only): ${currentBuild.currentResult}"
      archiveArtifacts artifacts: 'target/*.jar', onlyIfSuccessful: true
    }
    success { echo '✅ Pipeline succeeded.' }
    unstable { echo '🟡 Marked UNSTABLE (tests/quality gate).' }
    failure { echo '❌ Pipeline failed.' }
    aborted { echo '⏹️ Build was aborted.' }
  }
}
```

***

## 3) Setting / Controlling Build Result in **Declarative**

In Declarative, you typically **don’t** assign `currentBuild.result = '...'` directly. Instead, use the built-in steps below:

### A) Hard fail (mark **FAILURE** and stop)

```groovy
steps {
  error('Stopping: critical failure')
}
```

### B) Soft fail (keep running but mark **UNSTABLE**)

```groovy
steps {
  unstable('Unit test failures detected')
}
```

### C) Convert errors to a chosen result (continue running)

```groovy
steps {
  catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
    sh 'make test'    // non-zero exit allowed, build becomes UNSTABLE
  }
}
```

### D) Warn but don’t change overall result

```groovy
steps {
  warnError('Lint issues (not failing build)') {
    sh 'eslint . --max-warnings=0'  // non-zero -> warns, build stays SUCCESS
  }
}
```

> **Reading the result:**  
> Use `currentBuild.currentResult` (read-only).  
> Example: `echo "Now: ${currentBuild.currentResult}"`

> **Last resort:** If you *must* set `currentBuild.result` in Declarative, wrap it in a `script {}` block:

```groovy
steps {
  script {
    currentBuild.result = 'UNSTABLE'
  }
}
```

…but prefer `unstable()` / `catchError()` for clarity and portability.

***

## 4) Common Declarative Patterns

### Timeout around steps

```groovy
steps {
  timeout(time: 5, unit: 'MINUTES') {
    sh './long_running.sh'
  }
}
```

### Credentials (with and without environment)

```groovy
environment {
  // For single secret string credentials:
  NPM_TOKEN = credentials('npm-token-id')
}
steps {
  withCredentials([usernamePassword(credentialsId: 'git-creds', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
    sh 'git clone https://${GIT_USER}:${GIT_PASS}@github.com/org/repo.git'
  }
}
```

### Matrix builds

```groovy
stages {
  stage('Matrix Tests') {
    matrix {
      axes {
        axis {
          name 'OS'
          values 'linux', 'windows'
        }
        axis {
          name 'JDK'
          values '17', '21'
        }
      }
      agent { label "${OS}" }
      stages {
        stage('Test') {
          steps {
            sh "echo Testing on ${OS} with JDK ${JDK}"
          }
        }
      }
      post {
        always { echo "Done ${OS} / JDK ${JDK}" }
      }
    }
  }
}
```

### Shared library usage (Declarative)

```groovy
@Library('my-shared-lib@main') _
pipeline {
  agent any
  stages {
    stage('Use Lib') {
      steps {
        myLib.sayHello('Sammitra')
      }
    }
  }
}
```

***

## 5) Scripted vs Declarative—Quick Differences

*   **Declarative** = opinionated structure (`pipeline { agent ... stages { ... } post { ... } }`) with guardrails, better readability, and built-in blocks for conditions, options, and post-actions.
*   **Scripted** = free-form Groovy (`node { ... }`) giving you full control but less structure.

For status control:

*   **Declarative** → use `error`, `unstable`, `catchError`, `warnError`, and `post` conditions.
*   **Scripted** → you can directly set `currentBuild.result = '...'`.

***

## 6) Handy Snippets You’ll Reuse

**Retry & backoff**

```groovy
steps {
  retry(3) {
    sh 'curl -fS https://service/health'
    sleep time: 2, unit: 'SECONDS'
  }
}
```

**Parallel stages**

```groovy
stage('Parallel') {
  parallel {
    stage('Unit') { steps { sh 'make unit' } }
    stage('Integration') { steps { sh 'make it' } }
  }
}
```

**Conditional by branch**

```groovy
when { anyOf { branch 'main'; branch 'release/*' } }
```

***
