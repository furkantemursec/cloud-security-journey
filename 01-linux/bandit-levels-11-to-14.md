# OverTheWire: Bandit Walkthrough Notes (Levels 11–14)

## Level 11 -> Level 12: Decoding ROT13 Cipher (`tr`)

* **Goal:** Decode a Caesar/ROT13-shifted text string inside `data.txt` where every letter is rotated 13 places in the alphabet.
* **Command Used:**
  * `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`
* **Breakdown:**
  * `cat data.txt` : Output the shifted contents of `data.txt`.
  * `|` (Pipe) : Pass the text directly into `tr`.
  * `tr 'A-Za-z' 'N-ZA-Mn-za-m'` : Translate (`tr`) uppercase and lowercase letters by shifting them 13 positions across the 26-letter alphabet.
* **Key Takeaway:** Because the English alphabet has 26 letters, ROT13 is symmetrical—the exact same `tr` mapping both encodes and decodes text.

---

## Level 12 -> Level 13: Unpacking Hexdumps & Nested Archives (`xxd`, `gunzip`, `bunzip2`, `tar`)

* **Goal:** Convert a hexdump text file back into binary and extract multiple nested layers of compression (`gzip`, `bzip2`, `tar`) until reaching the plaintext password.
* **Commands Used:**
  * `mkdir /tmp/work && cp data.txt /tmp/work && cd /tmp/work`
  * `xxd -r data.txt > file_out`
  * `file file_out` *(repeated at every layer to inspect file headers)*
  * `gunzip`, `bunzip2`, and `tar -xf` *(used iteratively after renaming extensions)*
* **Breakdown:**
  * `xxd -r data.txt > file_out` : Reverse (`-r`) a hexadecimal dump back into a raw binary file.
  * `file file_out` : Inspect magic bytes/headers to identify the exact compression format at each stage.
  * `gunzip` / `bunzip2` / `tar -xf` : Decompress `.gz`, `.bz2`, and extract (`-x`) `.tar` archive files (`-f`).
* **Key Takeaway:** Linux decompression utilities rely on file extensions, whereas `file` reads actual file headers—always inspect unknown artifacts with `file` first inside `/tmp`.

---

## Level 13 -> Level 14: SSH Private Key Authentication (`ssh -i`)

* **Goal:** Log in as `bandit14` using a provided private SSH key instead of a password.
* **Command Used:**
  * `ssh -i bandit14.key bandit14@bandit.labs.overthewire.org -p 2220`
* **Breakdown:**
  * `ssh` : Initiate a secure shell connection.
  * `-i bandit14.key` : Specify the identity file (`-i` / private key) for key-based authentication.
  * `bandit14@bandit.labs.overthewire.org -p 2220` : Target user, host, and custom SSH port.
* **Key Takeaway:** Key-based SSH authentication (`ssh -i`) is the industry standard for accessing cloud instances (such as AWS EC2) and requires clean ASCII/UTF-8 formatting without BOM characters.

---

## Level 14 -> Level 15: Sending Data to Local Network Ports (`nc`)

* **Goal:** Submit the `bandit14` password to a service listening on `localhost` port `30000` to retrieve the next level's credentials.
* **Command Used:**
  * `cat /etc/bandit_pass/bandit14 | nc localhost 30000`
* **Breakdown:**
  * `cat /etc/bandit_pass/bandit14` : Read the current user's password from the system password store.
  * `|` (Pipe) : Stream the password directly into Netcat's standard input.
  * `nc localhost 30000` : Open a raw TCP socket connection using Netcat (`nc`) to `127.0.0.1` on port `30000`.
* **Key Takeaway:** Netcat (`nc`) acts as the TCP/IP Swiss Army knife for testing network services, banner grabbing, and interacting directly with open ports.
