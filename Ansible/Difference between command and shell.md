**Difference between `command` and `shell`.**




"Both command and shell modules are used to execute commands on remote hosts, but the key difference is that the command module executes commands directly without invoking a shell, while the shell module runs commands through the system shell (such as /bin/bash). Because command doesn't use a shell, it's more secure and should be the default choice. Use shell only when you need shell-specific features like pipes, redirection, environment variables, or command chaining."
