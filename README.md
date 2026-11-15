# Minishell

A small Unix shell written in **C** as part of the **42 curriculum**. This project explores what happens between typing a command and seeing its output: parsing input, expanding variables, creating processes, connecting pipes, and managing file descriptors.

## At a glance

- **Language:** C, compiled with `-Wall -Wextra -Werror`
- **Core concepts:** Unix processes, inter-process communication, signals, environment management, and manual memory management
- **Interface:** Interactive terminal prompt with GNU Readline and command history
- **Structure:** Separate modules for parsing, expansion, execution, built-ins, and utilities

## Features

- Execute external programs using `PATH` lookup or an explicit executable path.
- Connect multiple commands with pipes (`|`).
- Redirect input and output with `<`, `>`, and `>>`.
- Read heredoc input with `<<`.
- Handle single and double quotes, environment variables (`$HOME`), and the previous exit status (`$?`).
- Manage the shell environment with `export`, `unset`, and `env`.
- Handle terminal signals and end-of-input, including `Ctrl+C` and `Ctrl+D`.
- Track allocations through custom cleanup lists and clean up command resources between prompt iterations.

### Built-in commands

| Command | Purpose |
| --- | --- |
| `echo` | Print arguments, with support for `-n` |
| `cd` | Change the working directory |
| `pwd` | Print the current working directory |
| `export` | Display or set exported environment variables |
| `unset` | Remove environment variables |
| `env` | Display the environment |
| `exit` | Exit the shell |

## Build and run

The current Makefile is configured for **macOS with Homebrew**. It requires a C compiler, `make`, and GNU Readline.

From the repository directory:

```sh
brew install readline
make
./minishell
```

The shell displays an `mshell$>` prompt. Run it directly in a terminal with no arguments; the current implementation requires interactive input and does not accept scripts or a `-c` command option.

On other Unix systems, adapt the Makefile's `RLINC1` and `RLLIB1` settings to your local Readline installation.

| Target | Action |
| --- | --- |
| `make` | Build `minishell` |
| `make clean` | Remove object files |
| `make fclean` | Remove object files and the executable |
| `make re` | Rebuild from scratch |

## Try it

Enter these commands at the Minishell prompt:

```sh
echo "Hello from Minishell"
export PROJECT=Minishell
echo "$PROJECT"
echo '$PROJECT'
ls -l | wc -l
echo "Saved from Minishell" > output.txt
cat < output.txt
cat << EOF
Hello from a heredoc
EOF
echo $?
exit
```

The double-quoted variable expands; the single-quoted variable stays literal. The redirection example creates `output.txt` in the current directory.

## How it works

```text
Read input → Tokenize and validate → Parse and expand → Execute → Clean up
```

| Location | Responsibility |
| --- | --- |
| `mandatory/minishell.c` | Interactive loop, initialization, and per-command cleanup |
| `mandatory/parsing/` | Tokenization, quote handling, syntax checks, and command construction |
| `mandatory/expanding/` | Environment-variable, exit-status, and heredoc expansion |
| `mandatory/execution/` | Executable lookup, child processes, pipelines, redirections, heredocs, and signals |
| `mandatory/builtins/` | Built-in commands and shell-state updates |
| `mandatory/utils/` | String helpers, linked lists, allocation tracking, and resource cleanup |
| `includes/minishell.h` | Shared data structures and function declarations |

## Engineering focus

Building a shell brings several systems programming concerns together:

- **Process control:** launching programs, waiting for children, and propagating exit statuses.
- **File descriptor management:** connecting pipeline stages and routing command input and output.
- **Parsing and expansion:** preserving the meaning of quoted text while interpreting shell operators and variables.
- **State management:** keeping working-directory and environment changes within the shell session.
- **Resource lifetime:** managing dynamically allocated memory and open descriptors across repeated commands and error paths.

Minishell implements a focused subset of shell behavior for learning purposes; it is not a complete Bash implementation.

## Contributors

Developed by **sait-nac** and **kben-tou**, as credited in the source files.
