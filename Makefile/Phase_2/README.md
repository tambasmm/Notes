
# 📌 Phase 2 — Makefile Syntax Deep Dive

We will cover:

* Targets
* Rules
* Dependencies
* Variables
* Automatic variables (`$@`, `$<`, `$^`)
* Phony targets

---

# 🧱 1️⃣ Targets

## What is Target?

👉 Target = **What you want to build**

Example:

```
app: main.o add.o
```

Here:

```
app → Target
```

---

## Types of Targets

### 🎯 File Target

Creates file:

```
app
main.o
```

---

### 🎯 Special Target

Does action only:

```
clean
install
run
```

---

# 🧱 2️⃣ Rules (Core Structure)

---

## Rule Syntax

```
target: dependencies
	command
```

---

## Example

```
app: main.o add.o
	gcc main.o add.o -o app
```

Meaning:
👉 If main.o OR add.o changes → rebuild app

---

# 🧱 3️⃣ Dependencies (Prerequisites)

👉 Files needed to build target.

Example:

```
main.o: main.c add.h
```

Meaning:
👉 If main.c OR add.h changes → rebuild main.o

---

# 🧱 4️⃣ Variables (Makes Makefile Clean)

---

## Example

```
CC = gcc
CFLAGS = -Wall -g
```

Use:

```
$(CC) $(CFLAGS) main.c
```

---

## Why Variables Matter

Without variables:

```
gcc -Wall -g main.c
gcc -Wall -g add.c
gcc -Wall -g test.c
```

With variables:

```
$(CC) $(CFLAGS) main.c
```

Change once → update everywhere.

---

# 🧱 5️⃣ Automatic Variables (SUPER IMPORTANT 🔥)

These make Makefile powerful.

---

## 🟢 `$@` → Target Name

Example:

```
app: main.o add.o
	echo $@
```

Output:

```
app
```

---

## 🟢 `$<` → First Dependency

Example:

```
main.o: main.c
	echo $<
```

Output:

```
main.c
```

---

## 🟢 `$^` → All Dependencies

Example:

```
app: main.o add.o
	echo $^
```

Output:

```
main.o add.o
```

---

# 🧱 6️⃣ Pattern Rules (Avoid Repeating Code)

---

## Example Without Pattern Rule

```
main.o: main.c
	gcc -c main.c

add.o: add.c
	gcc -c add.c
```

---

## Same With Pattern Rule

```
%.o: %.c
	gcc -c $<
```

Meaning:
👉 Any `.o` from matching `.c`

---

# 🧱 7️⃣ Phony Targets

---

## Problem Without Phony

If file named `clean` exists → make may skip clean.

---

## Solution

```
.PHONY: clean
```

---

## Example

```
.PHONY: clean

clean:
	rm -f *.o app
```

---

# 🧪 Full Example (Intermediate Makefile)

```
CC = gcc
CFLAGS = -Wall -g

app: main.o add.o
	$(CC) $^ -o $@

%.o: %.c
	$(CC) $(CFLAGS) -c $<

.PHONY: clean
clean:
	rm -f *.o app
```

---

# 🧠 Read This Like English

```
Build app from all object files
Build any object file from matching C file
Clean removes build files
```

---

# ⭐ Most Used Things In Real Projects

🔥 Variables
🔥 Pattern rules
🔥 `$@ $< $^`
🔥 clean target

---

# 📊 Automatic Variable Cheat Sheet

| Variable | Meaning                            |
| -------- | -----------------------------------|
| `$@`     | Target name                        |
| `$<`     | First dependency                   |
| `$^`     | All dependencies                   |
| `$?`     | Only changed dependencies          |
| `$+`     | All dependencies (with duplicates) |
| `$*`     | Only newer dependencies            |

---

# 🧪 Mini Practice (Important)

If you see:

```
program: main.o util.o
	gcc $^ -o $@
```

What happens?

👉 `$^` = ?
👉 `$@` = ?

---

# 🚀 Phase 2 Mastery Checklist

If you know these → strong base:

☐ Rule syntax
☐ Target meaning
☐ Dependency meaning
☐ Variables usage
☐ `$@ $< $^` usage
☐ Pattern rules
☐ Phony targets

---

# 💡 One Line Summary

👉 **Phase 2 = Learning Makefile language grammar.**

---
