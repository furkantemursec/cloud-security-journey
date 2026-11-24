# OverTheWire: Bandit Walkthrough Notes (Levels 15–17)

## Level 15 -> Level 16: Encrypted SSL/TLS Connections (`openssl s_client`)

* **Goal:** Submit the current level's password to `localhost` on port `30001` using SSL/TLS encryption.
* **Command Used:**
  * `cat /etc/bandit_pass/bandit15 | openssl s_client -connect localhost:30001 -ign_eof`
* **Breakdown:**
  * `openssl s_client` : Open a command-line SSL/TLS client connection.
  * `-connect localhost:30001` : Target host and encrypted port.
  * `-ign_eof` : Ignore End-Of-File from piped input so the connection stays open long enough to receive the server's response.
* **Key Takeaway:** Unlike Netcat (`nc`) which sends plaintext traffic, `openssl s_client` handles encrypted TLS handshakes directly from the terminal.

---

## Level 16 -> Level 17: Port Scanning & Service Discovery (`nmap`)

* **Goal:** Scan `localhost` ports between `31000` and `32000`, identify which open port speaks SSL/TLS (`ssl/unknown`), and retrieve the `bandit17` private SSH key.
* **Commands Used:**
  * `nmap -T4 -p 31000-32000 localhost`
  * `nmap -sV -p 31046,31518,31691,31790,31960 localhost`
  * `cat /etc/bandit_pass/bandit16 | openssl s_client -connect localhost:31790 -ign_eof -quiet`
* **Breakdown:**
  * `nmap -T4 -p 31000-32000 localhost` : Perform a fast (`-T4`) port scan across the specified range on `localhost`.
  * `-sV` : Probe open ports to determine service/version info (distinguishing `echo` from `ssl/unknown`).
  * `-quiet` : Suppress certificate handshake output to cleanly capture the private key.
* **Key Takeaway:** Scanning for open ports first (`-T4`) and running service detection (`-sV`) only on discovered ports is significantly faster and standard reconnaissance practice.

---

## Level 17 -> Level 18: Comparing Files Line by Line (`diff`)

* **Goal:** Find the only line that changed between `passwords.old` and `passwords.new`.
* **Command Used:**
  * `diff passwords.old passwords.new`
* **Breakdown:**
  * `diff` : Compare two files line by line and output the differences (`<` indicates the old line, `>` indicates the new line).
* **Key Takeaway:** `diff` is essential for auditing configuration changes, log variations, and code modifications on Linux systems.
