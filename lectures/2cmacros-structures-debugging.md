---
theme: black
---
<style type="text/css">
div {
  font-size: clamp(16px, 3vw, 32px);
}
</style>

# 🛠️ Advanced C Programming Concepts (To be updated)

### Macros, Pointers, Memory Models & System Calls

---

## 🎯 Course Overview 
- **Key Topics**:
  - Preprocessor Directives & Macro Pitfalls
  - Pointer Arithmetic & Function Pointers
  - Memory Layout & Structure Packing
  - System Calls vs Library Functions
  - Common C Bugs & Debugging Techniques
- **Resources**:
  - [GCC Documentation](https://gcc.gnu.org/onlinedocs/)
  - [Structure Packing Guide](http://www.catb.org/esr/structure-packing/)

---

## 🧠 Preprocessor Magic

```c
#define MIN(X,Y) ((X) < (Y) ? (X) : (Y))
#define DEBUG_LOG(fmt, ...) \
    fprintf(stderr, fmt, __VA_ARGS__)
```

**Key Points**:
- Text substitution before compilation
- Common Pitfalls:
  - Operator precedence issues 
  - Multiple evaluation of arguments
  - Semicolon swallowing in multi-line macros

---

## 🎯 Pointer Essentials

```c
int arr[] = {1,2,3};
int *ptr = arr;
ptr += 2; // Now points to 3

// Void pointer example
void *data = malloc(100);
char *str = (char *)data;
```

**Pointer Arithmetic**:
- Type determines step size
- `char*` moves 1 byte, `int*` moves 4 bytes (typically)
- Void pointers can't be dereferenced directly

---

## 🧩 Structure Memory Layout

```c
struct Bad {
    char c;     // 1 byte
    int i;      // 4 bytes 
};              // Total 8 bytes (3 padding)

struct Good {
    int i;      // 4 bytes
    char c;     // 1 byte
};              // Total 5 bytes (1 padding)
```

**Alignment Rules**:
- Types align to their size (4-byte int → 4-byte alignment)
- Reorder fields to minimize padding
- Use `#pragma pack` for manual control (with caution!)

---

## 🖥️ System Call Mechanics

```nasm
; x86-64 write syscall
mov rax, 1     ; syscall number
mov rdi, 1     ; fd (stdout)
mov rsi, msg   ; buffer
mov rdx, len   ; length
syscall
```

**Key Differences**:
- User Mode → Kernel Mode transition
- Direct hardware interaction
- Error handling via errno
- Expensive context switches

---

## 🐞 Common C Bugs

```c
// Dangling pointer
int *p = malloc(sizeof(int));
free(p);
*p = 42; // CRASH!

// Buffer overflow
char buf[10];
strcpy(buf, "This is too long!");

// Uninitialized memory
int x;
printf("%d", x); // Undefined value
```

**Defense Strategies**:
- Always check malloc returns
- Use `strncpy` instead of `strcpy`
- Initialize variables explicitly
- Enable compiler warnings (-Wall -Wextra)

---

## 🔍 Debugging Tools

**GDB Cheat Sheet**:
```bash
gdb -q ./program
break main
run
next/step
print variable
backtrace
```

**Valgrind for Memory Leaks**:
```bash
valgrind --leak-check=full ./program
```

**Essential Commands**:
- `info registers` - View CPU registers
- `x/20wx $sp` - Examine stack memory
- `watch variable` - Set data breakpoints

---

## 🏁 Key Takeaways

1. Master preprocessor directives but avoid overuse
2. Understand pointer arithmetic and type casting
3. Optimize struct layouts for memory efficiency
4. Differentiate system calls from library functions
5. Use defensive programming to prevent common bugs
6. Leverage debugging tools (GDB/Valgrind) effectively


