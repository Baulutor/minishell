# Minishell

## 📚 Project Overview

**Minishell** is a project from [42 School](https://www.42.fr/) that consists of creating a simple shell interface that mimics the behavior of Bash in a minimalistic way. The goal is to deepen your understanding of how Unix shells work by implementing your own shell from scratch using system calls and low-level C programming.

This project focuses on:
- Learning process creation and management with `fork`, `execve`, and `wait`.
- Handling input/output redirection.
- Creating pipes to manage communication between processes.
- Implementing built-in commands internally.
- Managing environment variables.
- Implementing special features like `heredoc`.

## ✅ Features

### 🛠️ Shell Functionalities Implemented
- **Command execution** (with arguments)
- **Built-in commands**: `echo`, `cd`, `pwd`, `export`, `unset`, `env`, `exit`
- **Pipelines** using `|`
- **Redirections**:
  - Output redirection `>` and `>>`
  - Input redirection `<`
  - **Heredoc** support using `<<`
- **Environment variable expansion** using `$`
- **Quote handling**: single `'` and double `"` quotes
- **Exit status management**
- **Signal handling**: `Ctrl+C`, `Ctrl+\`, `EOF`

## 🧠 How It Works

### 🔄 Process Creation
- I use `fork()` to create child processes for executing non-built-in commands.
- In the child process, I use `execve()` to replace the process image with the command.
- The parent process waits for the child using `waitpid()`.

### 🧵 Piping
- When encountering a pipe `|`, I use `pipe()` to create a communication channel between two processes.
- File descriptors are duplicated using `dup2()` to redirect `stdin` and `stdout` appropriately between piped processes.

### 📂 Redirections
- For redirections like `>`, `>>`, and `<`, I use `open()` to access the file with the right mode (read, write, append).
- Then, I use `dup2()` to redirect file descriptors for `stdin` or `stdout` as needed before executing the command.

### 📜 Heredoc
- For `<<`, I read input until a delimiter is reached.
- The input is written to a temporary pipe, and the read end is connected to the command’s `stdin`.

### 🧾 Parsing
- A custom lexer/tokenizer was built to handle command strings with proper quote handling, escaping, and variable expansion.
- The parser converts the token list into an Abstract Syntax Tree (AST) or a simplified structure that can be executed sequentially or through recursion.

### 🧰 Built-ins
- Built-in commands are executed directly in the parent shell process (if no fork is needed) to allow for stateful behavior (e.g., `cd`, `export`, `unset`).

### 📛 Environment Variables
- A custom structure holds the environment variables, which can be modified during execution using `export` and `unset`.

### 📉 Error Handling
- System calls are checked for errors and appropriate error code is error messages are displayed.
- Syntax errors are caught during parsing and reported gracefully.

## 🚀 How to Run

```bash```
- make
- ./minishell

Then start typing commands like in a real shell:

minishell> echo Hello World
minishell> ls -l | grep minishell > file
minishell> cat << EOF
heredoc works!
EOF
