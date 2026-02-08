
# 🧪 Practical Demo — Small C Project

---

# 🔴 Step 1 — Manual Compilation (Old Way)

Run commands manually:

```
gcc -c main.c
gcc -c add.c
gcc main.o add.o -o app
```

Run program:

```
./app
```

Output:

```
Result = 8
```

---

## ❌ Problem with Manual Method

If you change only `add.c`
You must again run all commands manually.

---

# 🟢 Step 2 — Create Makefile

Create file named exactly:

```
Makefile
```

---

### 🧾 Makefile (Basic Version)

```
app: main.o add.o
	gcc main.o add.o -o app

main.o: main.c add.h
	gcc -c main.c

add.o: add.c add.h
	gcc -c add.c

clean:
	rm -f *.o app
```

⚠ Important: Before gcc line use **TAB**, not spaces.

---

# 🟢 Step 3 — Build Using Make

Run:

```
make
```

Make will run automatically:

```
gcc -c main.c
gcc -c add.c
gcc main.o add.o -o app
```

Run program:

```
./app
```

---

# 🔥 Step 4 — Dependency Magic Demo

Now edit `add.c`

Change:

```c
return a + b;
```

To:

```c
return a + b + 1;
```

---

## Now Run Again

```
make
```

### What Happens Now ✅

Output will be like:

```
gcc -c add.c
gcc main.o add.o -o app
```

Notice:
❌ main.c NOT compiled again
✅ Only changed file compiled

---

# 🧠 Why This Happened

Because Makefile knows:

```
add.o depends on add.c
```

So only rebuild needed part.

---

# 🧹 Step 7 — Clean Build Files

Run:

```
make clean
```

Removes:

```
*.o files
app executable
```

---

# 🧪 Mini Practice For You

Try this:
1️⃣ Add new file `sub.c`
2️⃣ Add function subtract
3️⃣ Update Makefile
4️⃣ Run make

---

# 🚀 Phase 1 Completed If You Can Answer This

If you change:
👉 `add.h`

What will rebuild?

Answer should be:
👉 main.o
👉 add.o
👉 app

(Because both depend on header)

