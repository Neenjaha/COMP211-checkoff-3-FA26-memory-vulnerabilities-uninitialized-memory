# Memory Vulnerabilities: Uninitialized Memory

The topic I chose to research is memory vulnerabilities with a focus on uninitialized memory. 
During my research I explored why C is specifically more vulnerable to certain memory vulnerabilities than other languages, what some of the defensive strategies for general memory vulnerabilities are, a specific example of an uninitialized memory vulnerability, and defensive methods to help prevent such a vulnerability from occurring. 

Why is C particularly vulnerable?
- C inherently trusts the programmer and assumes that the programmer knows what they are doing.
Because of this, many possible mistakes by the programmer are not detected or enforced in a way that would prevent such vulnerabilities, unlike other programming languages.

General Defense Techniques:
ASLR (Address Space Layout Randomization)
- Adds an offset when allocating memory which makes it harder for an attacker to find useful memory addresses.
Stack Canaries
- Generates a secret random value on the stack that, before returning, is checked to see if it has been changed. An attacker is not able to tell if a certain value is a canary or not and would be detected if they changed this value, and the program aborts.
Control-flow integrity
- At compile time, CFI determines where sections of code are allowed to jump to. CFI checks for indirect calls or jumps, if an attacker tries to call a function to or from an improper initial location in memory they will be detected.

Example of Uninitialized Memory attack:
Heartbleed
- an uninitialized memory vulnerability caused by a bug in a TLS plugin called "heartbeat," this was a simple program that accepted a payload from a user and would send back the same payload. However, this plugin assumed that the size of the payload designated by the user was the true size of the payload, which may have not been the case. This allowed an attacker to send a 1 byte payload but tell heartbeat that it was 65535 bytes, which, in turn, caused the server to send back 65534 extra bytes with uninitialized memory that the attacker may be able to find important information with.

Defense techniques for uninitialized memory:
- Prevention in code
  - Biggest takeaway = zero out memory to prevent leaking important information, the OS does this automatically upon allocating to a program, however any vulnerabilities create by the code are still possible leaks.
- Detection during development
  - Use tools like valgrind, compiler warnings (-Wall, -Wextra, etc.), and MemorySanitizer to make sure that no memory errors are occurring.
-  Language-level prevention
  - Certain languages (not C) have language level protection that prevents certain memory errors from successfully compiling.
