
## Tutorial: Manual Compilation

**Step 1: Compiling Source Files**

1. Open your terminal or command prompt.
2. Navigate to the directory where your main.c, module.c, and module.h files are located.

3. Compile the main.c file to produce the main object file (main.o) by running the following command:
   ```
   $ cc -I . -c main.c
   ```
   This command uses the cc compiler to compile main.c. The `-I .` flag includes the current directory as a header file location. The resulting object file, main.o, will be created in the same directory.

4. Compile the module.c file to produce the module object file (module.o) by running the following command:
   ```
   $ cc -I . -c module.c
   ```
   Similarly, this command compiles module.c and generates the module object file, module.o.

**Step 2: Linking Object Files**

1. Link the main.o and module.o files to produce the executable program by running the following command:
   ```
   $ cc main.o module.o -o prog
   ```
   This command uses the cc compiler to link the main.o and module.o files together. The resulting executable program, prog, will be created in the same directory.

**Step 3: Making the Program Executable**

1. Make the prog executable by running the following command:
   ```
   $ chmod +x prog
   ```
   This command adds the executable permission to the prog file, allowing you to run it as a program.

Congratulations! You have successfully manually compiled and generated the executable program using the provided main.c, module.c, and module.h files.

You can now run the executable program by executing the following command:
```
$ ./prog
```

Note: If you encounter any errors during the compilation or linking process, please double-check that all the required files are present in the directory and that there are no syntax or logical errors in the source code.
