## Reverse Engineering Tips

These are just things I've noticed about the workflows of those who I respect and have learned from.  I'm writing them here as a reminder to try to do these things personally.

 - **Always jump to an interesting point**; never start their analysis at the beginning of the file.   Use strings (like "Wrong password") and symbols to find the most interesting points to jump to.
 - **Focus on commonly vulnerable functions** (strcpy, memcpy, exec).  Gaining control of these functions often means you can alter memory, cause buffer overflows, and gain code execution.
 - **Look for functions that accept user-input** (recv, scanf, gets).  These functions often mean we can insert arbitrary data into memory and potentially supply unexpected values or longer-than-expected strings.
 - **Don't worry if someone solved something differently** than you.  There are usually many ways to solve a good CTF challenge.  CTF challenge writers are often surprised at a solution that they did not expect participants to find.
 - **Read writeups** to gain perspectives, learn techniques, and discover new tools.
 
## Things you should know

 - Basic blocks are chunks of assembly instructions that are broken apart by conditonal JMP instructions.  Based on the state of the CPU registers, these JMP instructions will take a different paths, so they're a logical place to break apart long streams of assembly instructions.
