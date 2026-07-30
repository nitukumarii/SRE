**Q: What does the $ symbol do in Bash?**


The $ symbol tells the shell to perform expansion. It is used to:

Access the value of variables (e.g., $name)
Access environment variables (e.g., $HOME)
Access special shell variables (e.g., $?, $$, $1)
Perform command substitution using $(command)



"$@" → Separate arguments.
"$*" → One combined string.



| Variable | Meaning                         | Example Output                    |
| -------- | ------------------------------- | --------------------------------- |
| `$0`     | Script name                     | `backup.sh`                       |
| `$1`     | First argument                  | `file.txt`                        |
| `$2`     | Second argument                 | `backup`                          |
| `$#`     | Number of arguments             | `2`                               |
| `$@`     | All arguments separately        | `file.txt backup`                 |
| `$*`     | All arguments as one string     | `file.txt backup`                 |
| `$$`     | Current process ID (PID)        | `12345`                           |
| `$?`     | Exit status of the last command | `0` (success), non-zero (failure) |
| `$HOME`  | User's home directory           | `/home/nitu`                      |
| `$PATH`  | Executable search path          | `/usr/bin:/bin:...`               |
