# OverTheWire: Bandit Walkthrough Notes (Levels 0–7)

## Level 0 -> Level 1: Reading Standard Files
- **Goal:** Connect via SSH and read the `readme` file in the home directory.
- **Commands Used:**
  - `ls` : List files in the current directory.
  - `cat readme` : Print the contents of the `readme` file to the terminal.
- **Key Takeaway:** Always inspect your landing directory first when accessing a Linux machine.

---

## Level 1 -> Level 2: Dashed Filenames (`-`)
- **Goal:** Read a file named `-` without the shell treating it as a command-line option/flag.
- **Command Used:**
  - `cat ./-` : Prepending `./` specifies the current directory path so the shell treats `-` as a literal filename.
- **Key Takeaway:** Special characters like `-` at the start of filenames require an explicit path (`./`).

---

## Level 2 -> Level 3: Filenames With Spaces
- **Goal:** Read a file containing spaces in its name (`--spaces in this filename--`).
- **Commands Used:**
  - `cat "./--spaces in this filename--"` : Wrapping the path in double quotes prevents the shell from splitting words by spaces.
  - *Alternative:* Using the `Tab` key to auto-escape spaces (`cat ./--spaces\ in\ this\ filename--`).
- **Key Takeaway:** Quotes or backslash escaping (`\`) are required when handling paths with whitespace.

---

## Level 3 -> Level 4: Hidden Files (`.` Prefix)
- **Goal:** Locate and read a hidden file inside the `inhere` directory.
- **Commands Used:**
  - `cd inhere` : Change into the `inhere` directory.
  - `ls -la` : List all files in long format, including hidden files starting with a dot (`.`).
- **Key Takeaway:** Security-sensitive configuration files in Linux (like `.env`, `.ssh`, `.bashrc`) start with `.` and are hidden from a standard `ls` command.

---

## Level 4 -> Level 5: Identifying File Types (`file`)
- **Goal:** Find the only human-readable ASCII text file among multiple binary/data files inside `inhere`.
- **Commands Used:**
  - `file ./*` : Inspect the true file type of every file in the current directory.
- **Key Takeaway:** Never rely solely on filenames or extensions; `file` identifies whether a file is plain text, executable, or raw data.

---

## Level 5 -> Level 6: Searching by Size & Permissions (`find`)
- **Goal:** Locate a human-readable, non-executable file of exact size `1033` bytes across nested subdirectories.
- **Command Breakdown:** `find . -type f -size 1033c ! -executable`
  - `find .` : Search recursively starting from the current directory (`.`).
  - `-type f` : Filter only regular files (`f`), ignoring directories.
  - `-size 1033c` : Match files with an exact size of `1033` bytes (`c` = bytes).
  - `! -executable` : Exclude executable files (`!` = logical NOT).
- **Key Takeaway:** `find` allows precise multi-condition filtering across deep directory trees.

---

## Level 6 -> Level 7: Server-Wide Search & Error Suppression (`2>/dev/null`)
- **Goal:** Find a `33`-byte file owned by user `bandit7` and group `bandit6` anywhere on the server.
- **Command Breakdown:** `find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null`
  - `find /` : Search across the entire filesystem starting from the root directory (`/`).
  - `-user bandit7 -group bandit6` : Filter by file owner and group ownership.
  - `2>/dev/null` : Redirect standard error (`stderr` = `2`) to `/dev/null` to hide "Permission denied" noise.
- **Key Takeaway:** Suppressing permission errors with `2>/dev/null` is essential when auditing whole Linux filesystems.

---

## Level 7 -> Level 8: Pattern Matching in Large Files (`grep`)
- **Goal:** Extract the password located next to the word `millionth` inside a massive text file (`data.txt`).
- **Command Used:**
  - `grep "millionth" data.txt` : Search `data.txt` for lines matching the string `"millionth"`.
- **Key Takeaway:** `grep` is the core tool for filtering massive system and security log files quickly.
