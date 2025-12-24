
***

## 1) Minimal Scripted Pipeline

```groovy
node {
  stage('Build') {
    echo 'Building...'
    sh 'make build'
  }
}
```

*   `node { ... }` allocates an executor/agent.
*   Stages are **optional** in Scripted, but recommended for UI visualization.

***

## 2) Canonical Structure (Cheat Sheet)

```groovy
// Optionally load shared library first
@Library('my-shared-lib@main') _

timestamps {
  node('linux && docker') { // label selection
    // Global env for this node block
    withEnv(["APP_ENV=dev"]) {

      // Parameters are exposed as 'params' only in multibranch/Declarative;
      // in Scripted standalone jobs you typically define job parameters and access: params.MY_PARAM
      stage('Checkout') {
        checkout scm
      }

      stage('Build') {
        // Conditions in Scripted are regular Groovy 'if' statements
        if (env.BRANCH_NAME != 'main') {
          echo "Non-main build"
        }
        sh 'mvn -B -U -DskipTests package'
      }

      stage('Test') {
        // Control result without stopping pipeline
        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
          sh 'mvn test'
          junit 'target/surefire-reports/*.xml'
        }
      }

      stage('Quality Gates (Parallel)') {
        parallel(
          Lint: {
            sh 'npm run lint'
          },
          SAST: {
            sh './scan.sh'
          }
        )
      }

      stage('Deploy') {
        if (env.BRANCH_NAME == 'main') {
          timeout(time: 10, unit: 'MINUTES') {
            input message: 'Deploy to PROD?', ok: 'Deploy'
          }
          sh './deploy.sh'
        } else {
          echo "Skipping deploy for ${env.BRANCH_NAME}"
        }
      }
    }
  }
}
```

**Notes:**

*   Scripted uses **plain Groovy** control flow (`if`, `try/catch`, loops).
*   You can nest anything, but keep it clean for readability.

***

## 3) Controlling Build Result in **Scripted**

Scripted gives you **full, direct control**.

### A) Directly set the overall result

```groovy
currentBuild.result = 'UNSTABLE'  // or 'FAILURE', 'ABORTED', 'SUCCESS', 'NOT_BUILT'
```

### B) Hard fail immediately

```groovy
error('Critical failure')  // throws, marks FAILURE unless caught
```

### C) Soft fail (continue but mark UNSTABLE)

```groovy
unstable('Unit test failures detected')
```

### D) Convert errors to a chosen result and keep going

```groovy
catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
  sh 'make test'  // non-zero exit -> build UNSTABLE, stage shown as FAILURE
}
```

### E) Read status at any time

```groovy
echo "Status now: ${currentBuild.currentResult}"  // read-only computed
// Versus:
echo "Manual result (may be null): ${currentBuild.result}"
```

**Lifecycle tip:** At start, `currentBuild.result == null`, `currentBuild.currentResult == 'SUCCESS'`.

***

## 4) Emulating Declarative `post {}` in Scripted

Scripted has no `post` block. Use `try/finally` and status checks:

```groovy
node {
  try {
    stage('Build') {
      sh 'make build'
    }
    stage('Test') {
      sh 'make test'
    }
  } catch (err) {
    echo "Caught error: ${err}"
    // Optionally override result
    // currentBuild.result = 'FAILURE'
    throw err  // rethrow if you want the job to fail
  } finally {
    echo "Final status: ${currentBuild.currentResult}"
    archiveArtifacts artifacts: 'target/*.jar', onlyIfSuccessful: true
    if (currentBuild.currentResult == 'UNSTABLE') {
      echo '🟡 Tests or quality gate marked build UNSTABLE'
    }
  }
}
```

***

## 5) Timeouts, Retries, and Backoff

```groovy
node {
  stage('Ping service with retry') {
    retry(3) {
      timeout(time: 10, unit: 'SECONDS') {
        sh 'curl -fS https://service/health'
      }
      sleep time: 2, unit: 'SECONDS'
    }
  }
}
```

***

## 6) Credentials and Environment

### A) withCredentials

```groovy
node {
  withCredentials([
    usernamePassword(credentialsId: 'git-creds', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS'),
    string(credentialsId: 'npm-token-id', variable: 'NPM_TOKEN')
  ]) {
    sh 'git clone https://${GIT_USER}:${GIT_PASS}@github.com/org/repo.git'
    sh 'echo "//registry.npmjs.org/:_authToken=${NPM_TOKEN}" > ~/.npmrc'
  }
}
```

### B) withEnv

```groovy
node {
  withEnv(["JAVA_HOME=${tool 'temurin-17'}", "PATH+JAVA=${tool 'temurin-17'}/bin"]) {
    sh 'java -version'
  }
}
```

***

## 7) Tools, ANSI color, and Options (Scripted)

*   **Tools**: use `tool 'name'` then add to `PATH` via `withEnv` as shown above.
*   **ANSI color**:
    ```groovy
    ansiColor('xterm') {
      sh 'echo "\u001b[32mGreen text\u001b[0m"'
    }
    ```
*   **Build options** are configured via **job settings** or `properties` step:
    ```groovy
    properties([
      buildDiscarder(logRotator(numToKeepStr: '20')),
      disableConcurrentBuilds(),
      pipelineTriggers([cron('H/15 * * * *')])
    ])
    ```

***

## 8) Parallel, Fan-out/Fan-in Patterns

```groovy
node {
  stage('Parallel') {
    def tasks = [:]
    ['linux', 'windows'].each { os ->
      tasks[os] = {
        node(os) {
          stage("Test on ${os}") {
            sh "echo Testing on ${os}"
          }
        }
      }
    }
    parallel tasks
  }

  stage('Fan-in') {
    echo 'Aggregate reports or deploy after all branches finish'
  }
}
```

***

## 9) Matrix-like Behavior (DIY in Scripted)

Scripted doesn’t have a `matrix` block; build your own:

```groovy
node {
  def axes = []
  ['linux','windows'].each { os ->
    ['17','21'].each { jdk ->
      axes << [os: os, jdk: jdk]
    }
  }

  def branches = axes.collectEntries { axis ->
    ["${axis.os}_jdk${axis.jdk}": {
      node(axis.os) {
        stage("Env ${axis.os} / JDK ${axis.jdk}") {
          withEnv(["JAVA_HOME=${tool "temurin-${axis.jdk}"}", "PATH+JAVA=${tool "temurin-${axis.jdk}"}/bin"]) {
            sh "echo Running on ${axis.os} with JDK ${axis.jdk}"
          }
        }
      }
    }]
  }

  stage('Matrix') { parallel branches }
}
```

***

## 10) Shared Libraries in Scripted

```groovy
@Library('my-shared-lib@main') _
node {
  stage('Use Library') {
    myLib.sayHello('Sammitra')
    myLib.buildAndTest() // custom vars or classes from the library
  }
}
```

***

## 11) Practical Templates

### A) Maven/JUnit template

```groovy
timestamps {
  node('linux && maven') {
    properties([
      buildDiscarder(logRotator(numToKeepStr: '30')),
      disableConcurrentBuilds()
    ])

    try {
      stage('Checkout') { checkout scm }

      stage('Build') {
        withEnv(["JAVA_HOME=${tool 'temurin-17'}","PATH+JAVA=${tool 'temurin-17'}/bin"]) {
          sh 'mvn -B -U -DskipTests package'
        }
      }

      stage('Test') {
        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
          sh 'mvn test'
          junit '**/target/surefire-reports/*.xml'
        }
      }

      stage('Package') {
        archiveArtifacts artifacts: 'target/*.jar', onlyIfSuccessful: true
      }
    } finally {
      echo "Final: ${currentBuild.currentResult}"
    }
  }
}
```

### B) Node.js/Lint/Test template

```groovy
node('linux && node') {
  stage('Checkout') { checkout scm }
  stage('Install') { sh 'npm ci' }

  stage('Lint') {
    warnError('Lint warnings (not failing build)') {
      sh 'npm run lint'
    }
  }

  stage('Test') {
    catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
      sh 'npm test -- --ci --reporters=jest-junit'
      junit 'junit.xml'
    }
  }

  stage('Build') { sh 'npm run build' }

  stage('Publish') {
    if (env.BRANCH_NAME == 'main') {
      withCredentials([string(credentialsId: 'npm-token', variable: 'NPM_TOKEN')]) {
        sh 'npm config set //registry.npmjs.org/:_authToken=$NPM_TOKEN'
        sh 'npm publish'
      }
    } else {
      echo "Skipping publish on ${env.BRANCH_NAME}"
    }
  }
}
```

***

## 12) Best Practices (Scripted)

*   Keep **stage boundaries** clear for UI visibility.
*   Centralize **result management**: use `catchError`/`unstable` in test/quality stages; avoid random `currentBuild.result` writes across the script.
*   Use `try/catch/finally` to **ensure cleanup** and emulate Declarative `post`.
*   Prefer **labels** on `node()` and keep shell steps **idempotent**.
*   For large pipelines, encapsulate logic in **shared libraries**.

***
