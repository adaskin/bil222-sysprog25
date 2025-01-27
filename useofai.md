## Use of AI, GPT, Gemini, DeepSeek, etc.
You are free to use them... However,over-reliance may prevent you from thinking 
Below is example guide how you can leverage such tools for this course(taken from DeepSeek R1) 
* You can use AI's capabilities for explanations, code examples, debugging, and interactive learning.

### Example use and prompts:
 - **Unix Basics** generate shell script examples (e.g., `ls -l` analysis)
 - **System Calls & C Programming**  `open`, `read`, `write`, `close`, `fork`, `exec`, `wait`, `signal`, etc. 
   - Ask for code snippets (e.g., "Write a C program using `fork()`"). 
   - Explain error handling (e.g., "Why does `fork()` return -1?"). 
- **Process Management** Process creation, termination, zombie processes. 
   - Simulate process trees. 
   - Debug infinite loops or orphaned processes. 
- **Inter-Process Communication (IPC)** Pipes, FIFOs, shared memory, message queues. 
   - Generate code for producer-consumer problems. 
   - Explain race conditions and synchronization. 
 - **Signals** Signal handlers, masking, `sigaction`. 
   - Demonstrate safe signal handling practices. 
   - Debug issues like signal interruption of system calls. 
- **Networking & Sockets** TCP/UDP sockets, `select`, `poll`. 
   - Create a simple client-server chat example. 
   - Troubleshoot "Address already in use" errors. 
  
  #### **General prompts related to Lectures & Theory** 
  - Use AI to find resources
  - Is this use of `select()` efficient?
  - What's the difference between `fork()` and `vfork()`?
  #### **Prompts for Code Examples** 
  - Write a C program that uses `pipe()` to send data between parent and child processes.
  ####  **Labs & Assignments** 
  You should write your code yourself. But, you can take help from AI.
  - Check the correct use: e.g. Is this use of `select()` efficient?
  - You can use AI to generate test cases (e.g., for a task of copying files, test cases that use empty files, large files).
  - You can analyze code dumps, errors, etc. Pair AI with `gdb`, `valgrind`, and `strace`
    - e.g. prompt: Why does this code create a zombie process? 
    - Explain `strace`/`gdb` outputs. P
    - e.g. I’m getting ‘Broken Pipe’ errors in my client-server program. How do I fix it?
