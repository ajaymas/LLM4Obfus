
# ✅ Compiling `hello_world.c` with All Possible Optimization Levels on Linux (GCC)

---

## 🔥 1. Prepare the C Program

✅ **Create a simple `hello_world.c` file:**
```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

✅ **Save the file:**
```bash
nano hello_world.c
```
Paste the code and save (`CTRL + X`, then `Y`, then `ENTER`).

---

## 🔥 2. Compile with All GCC Optimization Levels

GCC offers several optimization levels:
- `-O0`: No optimization.  
- `-O1`: Basic optimization.  
- `-O2`: Moderate optimization.  
- `-O3`: Aggressive optimization.  
- `-Os`: Optimize for size.  
- `-Ofast`: Aggressive optimizations, including `-O3` and unsafe math optimizations.  
- `-Og`: Optimizes for debugging (similar to `-O1` but with better debugging support).  
- `-Oz`: Optimizes for **smallest code size** (Clang only, but you can use `-Os` in GCC).  
- `-fprofile-generate` and `-fprofile-use`: Profile-guided optimizations.  
- **LTO (Link Time Optimization)**: Cross-file optimization (`-flto`).  
- **PGO (Profile-Guided Optimization)**: Runtime profile-based optimization.  

---

## ✅ 3. Compilation Commands

Run the following commands to compile with **all optimization levels**:

```bash
# No optimization
gcc -O0 -o hello_O0 hello_world.c

# Basic optimization
gcc -O1 -o hello_O1 hello_world.c

# Moderate optimization
gcc -O2 -o hello_O2 hello_world.c

# Aggressive optimization
gcc -O3 -o hello_O3 hello_world.c

# Optimize for size
gcc -Os -o hello_Os hello_world.c

# Fastest optimization (unsafe floating-point operations)
gcc -Ofast -o hello_Ofast hello_world.c

# Debug-friendly optimization
gcc -Og -o hello_Og hello_world.c

# Link Time Optimization (LTO)
gcc -O3 -flto -o hello_LTO hello_world.c

# Profile-Guided Optimization (PGO)
# Step 1: Generate profile data
gcc -fprofile-generate -o hello_pgo_gen hello_world.c
./hello_pgo_gen  # Run to generate profiling data

# Step 2: Compile with profile-guided optimization
gcc -fprofile-use -o hello_PGO hello_world.c
```

---

## ✅ 4. Verify the Compilation
List the compiled binaries:
```bash
ls -lh hello_*
```

✅ You should see multiple binaries, each compiled with a different optimization level:
```
-rwxr-xr-x 1 user user 8.0K hello_O0  
-rwxr-xr-x 1 user user 7.5K hello_O1  
-rwxr-xr-x 1 user user 6.9K hello_O2  
-rwxr-xr-x 1 user user 6.8K hello_O3  
-rwxr-xr-x 1 user user 5.5K hello_Os  
-rwxr-xr-x 1 user user 5.3K hello_Ofast  
-rwxr-xr-x 1 user user 7.2K hello_Og  
-rwxr-xr-x 1 user user 6.2K hello_LTO  
-rwxr-xr-x 1 user user 6.0K hello_PGO  
```

---

## ✅ 5. Test the Binaries
Run each binary to confirm they work:
```bash
./hello_O0
./hello_O1
./hello_O2
./hello_O3
./hello_Os
./hello_Ofast
./hello_Og
./hello_LTO
./hello_PGO
```

✅ Output:
```
Hello, World!
```

---

## ✅ 6. Compare the Binary Sizes
You can compare the **binary sizes** with:
```bash
ls -lh hello_*
```

✅ Example Output:
```
-rwxr-xr-x 1 user user 8.0K hello_O0  
-rwxr-xr-x 1 user user 7.5K hello_O1  
-rwxr-xr-x 1 user user 6.9K hello_O2  
-rwxr-xr-x 1 user user 6.8K hello_O3  
-rwxr-xr-x 1 user user 5.5K hello_Os  
-rwxr-xr-x 1 user user 5.3K hello_Ofast  
-rwxr-xr-x 1 user user 7.2K hello_Og  
-rwxr-xr-x 1 user user 6.2K hello_LTO  
-rwxr-xr-x 1 user user 6.0K hello_PGO  
```

---

## ✅ 7. Analyze the Binary with `file` and `objdump`

✅ **Check the binary details:**
```bash
file hello_O3
```
Example Output:
```
hello_O3: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked
```

✅ **View assembly code for all binaries:**
```bash
objdump -d hello_O3 | less
```

✅ **Compare optimization effects:**
- **O0** → More lines, no optimizations.  
- **O3** → Fewer lines, inlined functions, and loop unrolling.  
- **Os/Ofast** → Smaller binary size.  

---

## ✅ 8. Conclusion
1. **You have now compiled `hello_world.c` with all major GCC optimizations**.  
2. **Optimization levels** control binary size, performance, and complexity.  
3. **Profile-guided optimization (PGO)** and **Link Time Optimization (LTO)** add advanced optimizations.  
4. You can **analyze the binaries with `objdump`** to observe the differences in assembly code.  
5. Use the **binaries for decompilation testing** with **LLM4Decompile** and **DeGPT** to evaluate their decompilation accuracy across different optimization levels.  

✅ Let me know if you need **further experimentation, additional compiler flags**, or testing scripts! 🚀



# ✅ Applying Binary Stripping Techniques During Compilation on Linux (GCC)

---

## 🔥 1. What is Binary Stripping?
**Binary stripping** is the process of removing symbols, debugging information, and metadata from compiled binaries. This makes the binaries:

- **Smaller in size.**
- **Harder to reverse engineer.**
- **More challenging for LLM-based decompilers** (like **LLM4Decompile** and **DeGPT**) to recover meaningful code.

✅  **Types of Stripping Techniques:**  
- **Removing Debug Symbols** â†’ Strips symbols used for debugging.  
- **Removing All Symbols** â†’ Strips all symbols (including function names).  
- **Removing Specific Sections** â†’ Strips individual sections like `.text`, `.data`, `.bss`, etc.  
- **Using Compiler Flags** â†’ Instructs the compiler to strip during compilation.  

---

## 🔥  2. Stripping Techniques with Commands

### ✅ 2.1. Compile with Debug Symbols
First, compile the binary with **debugging symbols** (`-g` flag).

✅ **Command:**
```bash
# Compile with debug symbols
gcc -g -o hello_dbg hello_world.c
```

✅  **Check for symbols:**
```bash
nm hello_dbg
```
**Output Example:**
```
0000000000401130 T main  
0000000000401140 t frame_dummy  
0000000000401160 T __libc_csu_init  
0000000000401200 T __libc_csu_fini  
```
â†’ You will see the **function symbols and other debug information**.

---

### ✅  2.2. Strip Debug Symbols Only

✅  **Command:**
```bash
# Strip debug symbols only
strip --strip-debug hello_dbg -o hello_stripped_debug
```

✅  **Check the symbols again:**
```bash
nm hello_stripped_debug
```
âœ… **Output:**
```
(no symbols)
```
→  **Effect:** Removes only **debug symbols**, keeping function names intact.

---

### ✅  2.3. Strip All Symbols

✅ **Command:**
```bash
# Strip all symbols
strip --strip-all hello_dbg -o hello_stripped_all
```

✅ **Check the symbols:**
```bash
nm hello_stripped_all
```
✅ **Output:**
```
(no symbols)
```
→ **Effect:** Removes **all symbols**, making it much harder to reverse engineer.

---

### ✅  2.4. Strip Specific Sections
You can **strip individual sections** (e.g., `.symtab`, `.debug`, `.bss`, etc.).

âœ… **List all sections:**
```bash
readelf -S hello_dbg
```
**Example Output:**
```
[Nr] Name      Type            Address          Offset  
[ 1] .text     PROGBITS        0000000000401000  0x1000  
[ 2] .data     PROGBITS        0000000000601000  0x2000  
[ 3] .bss      NOBITS          0000000000602000  0x3000  
[ 4] .symtab   SYMTAB          0000000000000000  0x4000  
[ 5] .strtab   STRTAB          0000000000000000  0x5000  
```

**Strip `.symtab` and `.strtab`:**
```bash
objcopy --remove-section=.symtab --remove-section=.strtab hello_dbg hello_custom_strip
```

**Verify sections:**
```bash
readelf -S hello_custom_strip
```
**Effect:** Removes the **symbol table** and **string table**, making the binary harder to analyze.

---

### 2.5. Strip During Compilation (GCC Flags)
You can instruct **GCC to automatically strip symbols** during the compilation process by adding the `-s` flag.

**Command:**
```bash
# Compile and strip simultaneously
gcc -s -o hello_stripped hello_world.c
```

 **Verify the binary:**
```bash
nm hello_stripped
```
 **Output:**
```
(no symbols)
```

 **Effect:**  
- The binary is **smaller and stripped** of symbols.  
- **No need for manual stripping** after compilation.  

---

### âœ… 2.6. Strip with Optimization Levels
You can **combine stripping with different optimization levels** during compilation.

 **Command Examples:**
```bash
# Compile with -O3 and strip all symbols
gcc -O3 -s -o hello_O3_stripped hello_world.c

# Compile with -Os (size optimization) and strip
gcc -Os -s -o hello_Os_stripped hello_world.c
```

 **Check the size:**
```bash
ls -lh hello_*
```
**Effect:**  
- The binaries will be **smaller and harder to reverse engineer**.  
- **LLM-based decompilers** will struggle with stripped binaries.  

---

## 3. Combine Stripping + Obfuscation
To **further harden binaries**:

- **Compile with optimization + stripping.**
- **Apply obfuscation before stripping.**

 **Example Commands:**
```bash
# Compile with obfuscation (LLVM)
clang -mllvm -fla -o obf_test hello_world.c

# Strip symbols
strip --strip-all obf_test -o obf_stripped
```
 **Effect:**  
- **Obfuscation** makes the **control flow complex**.  
- **Stripping** removes symbols.  
- **LLM-based decompilers** will struggle to recover meaningful code.  

---

## 4. Automate Stripping for Multiple Binaries
If you want to **automate stripping for multiple binaries**:

- Create a directory with multiple binaries: `/binaries`.  
- Use the following script:  

 **Script:**
```bash
#!/bin/bash

# Input/Output Folders
BIN_DIR="./binaries"
STRIPPED_DIR="./stripped_binaries"
mkdir -p "$STRIPPED_DIR"

# Iterate through each binary and strip symbols
for binary in "$BIN_DIR"/*; do
    base_name=$(basename "$binary")
    
    # Strip debug symbols
    strip --strip-debug "$binary" -o "$STRIPPED_DIR/${base_name}_strip_dbg"

    # Strip all symbols
    strip --strip-all "$binary" -o "$STRIPPED_DIR/${base_name}_strip_all"

    echo "Stripped $binary and saved in $STRIPPED_DIR"
done
```

 **Usage:**
```bash
chmod +x strip_binaries.sh
./strip_binaries.sh
```

 **Effect:**  
- Automatically **strips all binaries** in the `/binaries` directory.  
- Saves **stripped versions** in `/stripped_binaries`.  

---

## ðŸ”¥ 5. Conclusion
 You now have multiple techniques to apply **binary stripping** during compilation:

1. **Manual stripping** with `strip` and `objcopy`.  
2. **GCC flags (`-s`)** to strip during compilation.  
3. **Selective stripping of sections** for custom protection.  
4. **Combining stripping with obfuscation** for stronger protection.  
5. **Automating stripping** for multiple binaries.  

 You can now test the stripped binaries against **LLM4Decompile** and **DeGPT** to **evaluate decompilation accuracy** on hardened binaries! ðŸš€

