# OverTheWire: Bandit Walkthrough Notes (Levels 8–10)

## Level 8 -> Level 9: Finding Unique Lines (`sort` & `uniq`)
- **Goal:** Locate the single line of text inside `data.txt` that occurs only once among thousands of repeated lines.
- **Command Used:**
  - `sort data.txt | uniq -u`
- **Breakdown:**
  - `sort data.txt` : Arrange all lines alphabetically so identical lines sit adjacent to each other.
  - `|` (Pipe) : Pass the standard output of `sort` directly as standard input into `uniq`.
  - `uniq -u` : Print only unique (`-u`) lines that do not repeat.
- **Key Takeaway:** `uniq` only detects duplicates when they are adjacent, so `sort` must almost always precede `uniq` during log analysis.

---

## Level 9 -> Level 10: Extracting Readable Strings From Binary Data (`strings`)
- **Goal:** Extract human-readable strings preceded by several `=` characters inside a binary/corrupted `data.txt` file.
- **Command Used:**
  - `strings data.txt | grep "==="`
- **Breakdown:**
  - `strings data.txt` : Filter out non-printable binary bytes and extract only human-readable ASCII character sequences.
  - `| grep "==="` : Narrow down the extracted strings to lines containing `===`.
- **Key Takeaway:** `strings` is a staple forensic and malware-triage command for inspecting non-text binaries.

---

## Level 10 -> Level 11: Decoding Base64 Data (`base64`)
- **Goal:** Decode Base64-encoded text stored inside `data.txt` to retrieve the next password.
- **Command Used:**
  - `base64 -d data.txt`
- **Breakdown:**
  - `base64 -d` : Decode (`-d`) Base64-encoded data back into plain text.
- **Key Takeaway:** Base64 is an encoding format (not encryption) frequently used in web tokens, API headers, and Kubernetes Secrets.
