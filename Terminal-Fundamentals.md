# Terminal Fundamentals

**Coddy Terminal journey, Section 1 (Computer Networking path, Step 1)** · 15 chapters · 82 lessons · 80 graded challenges · Certificate earned 2026-11-05 ([verify](https://coddy.tech/certifications/tj6FfM-terminal-JRk84c))

[← Back to portfolio summary](README.md)

> **About the commands below:** Coddy's browser terminal grades each challenge live but does not keep a copy of what was typed. The commands here are reference solutions I wrote afterward for each challenge I passed. They produce the graded output, with notes on what each one does. Two lessons (Comments, File Ownership) are concept-only and have no graded challenge.

## What this course covers

Linux command-line fluency: navigation and paths, file and directory operations, wildcards, reading and filtering text (`head`, `tail`, `wc`, `sort`, `uniq`, `grep`), redirection and pipes, permissions (`chmod`) and ownership, environment variables and `PATH`, and Bash scripting (variables, input, `if`, `for`, `while`). Along the way there are three projects (Organize Photos, Log Analyzer, Backup Script) and three final scripting challenges.

## Contents

1. [Introduction](#1-introduction)
2. [Navigation](#2-navigation)
3. [Files](#3-files)
4. [Directories](#4-directories)
5. [Organize Photos Project](#5-organize-photos-project)
6. [Wildcards And Patterns](#6-wildcards-and-patterns)
7. [File Content](#7-file-content)
8. [Redirection](#8-redirection)
9. [Piping](#9-piping)
10. [Log Analyzer Project](#10-log-analyzer-project)
11. [Permissions](#11-permissions)
12. [Environment](#12-environment)
13. [Shell Scripting Basics](#13-shell-scripting-basics)
14. [Backup Script Project](#14-backup-script-project)
15. [Final Challenges](#15-final-challenges)
16. [Command cheat sheet](#command-cheat-sheet)

---

## 1. Introduction

### 1.1 What Is The Terminal · Easy
**Task:** Run a pre-typed command.
```bash
echo "Hello World!"
```

### 1.2 Your First Command · Beginner
**Task:** Print text with `echo`.
```bash
echo "Hello World!"
```

### 1.3 Comments · Concept lesson (no graded challenge)
```bash
# Everything after a # is ignored by the shell
echo "Hello!"   # inline comments work too
```

---

## 2. Navigation

### 2.1 Print Working Directory · Beginner
**Task:** Show the current directory.
```bash
pwd
```

### 2.2 List Files · Beginner
**Task:** List the visible files.
```bash
ls
```
Use `ls -a` to also show hidden dotfiles.

### 2.3 Change Directory · Beginner
**Task:** Move into `documents` and confirm.
```bash
cd documents
pwd
```

### 2.4 Absolute vs Relative Paths · Beginner
**Task:** Use a relative path to reach `documents`.
```bash
cd documents        # relative (from where I am)
pwd                 # /tmp/user/home/documents (absolute)
```

### 2.5 Home And Root Directory · Beginner
**Task:** Go to the filesystem root.
```bash
cd /
pwd
```
`cd` on its own (or `cd ~`) returns home.

### 2.6 Recap: Find Your Way · Easy
**Task:** Go into `documents`, list it, and print the location.
```bash
cd documents
ls
pwd
```

---

## 3. Files

### 3.1 Create A File · Beginner
**Task:** Create `hello.txt` and confirm it exists.
```bash
touch hello.txt
ls
```

### 3.2 Read A File · Beginner
**Task:** Print `readme.txt`.
```bash
cat readme.txt
```

### 3.3 Copy A File · Beginner
**Task:** Back up `readme.txt` to `backup.txt`.
```bash
cp readme.txt backup.txt
ls
```

### 3.4 Move And Rename A File · Beginner
**Task:** Rename `readme.txt` to `info.txt`.
```bash
mv readme.txt info.txt
ls
```

### 3.5 Delete A File · Beginner
**Task:** Delete `readme.txt`.
```bash
rm readme.txt
ls
```
There is no recycle bin in the terminal, so `rm` is permanent.

### 3.6 Recap: File Operations · Easy
**Task:** Create, copy, move and delete in sequence.
```bash
touch newfile.txt
cp newfile.txt backup.txt
mv backup.txt documents/
rm newfile.txt
ls
ls documents
```

---

## 4. Directories

### 4.1 Create A Directory · Beginner
```bash
mkdir projects
ls
```

### 4.2 Copy A Directory · Beginner
```bash
cp -r documents documents_backup
ls
```
`-r` (recursive) is required to copy a folder and everything in it.

### 4.3 Move And Rename A Directory · Beginner
```bash
mv documents files
ls
```

### 4.4 Delete A Directory · Beginner
```bash
rm -r documents
ls
```

### 4.5 Recap: Directory Operations · Easy
**Task:** Run exactly six commands, in order.
```bash
mkdir projects
cp -r documents projects/documents_backup
mv projects/documents_backup projects/archive
rm -r documents
ls
ls projects
```

---

## 5. Organize Photos Project

Sort a folder of loose photos into `nature/`, `travel/` and `family/`.

### 5.1 Project Overview · Beginner
```bash
ls
```

### 5.2 Creating The Structure · Easy
```bash
mkdir nature travel family
ls
```

### 5.3 Moving The Files · Easy
```bash
mv beach.jpg mountain.jpg sunset.jpg nature/
mv paris.jpg travel/
mv family.jpg family/
ls nature
ls travel
ls family
```

### 5.4 Cleaning Up · Medium
**Task:** Rename one photo, copy the notes into every folder, and remove the loose original.
```bash
mv nature/beach.jpg nature/beach_edited.jpg
cp notes.txt nature/
cp notes.txt travel/
cp notes.txt family/
rm notes.txt
ls
ls nature
ls travel
ls family
```

---

## 6. Wildcards And Patterns

### 6.1 The Star Wildcard · Beginner
```bash
ls *.jpg
```
`*` matches any number of characters.

### 6.2 The Question Mark Wildcard · Beginner
```bash
ls file?.txt
```
`?` matches exactly one character, so `file10.txt` is excluded.

### 6.3 Bracket Wildcards · Beginner
```bash
ls file[1-3].txt
```

### 6.4 Combining Wildcards · Easy
```bash
ls photo_[0-9].jpg
```
This matches `photo_1.jpg` and `photo_2.jpg` but not `photo_10.jpg` or any `.png`.

### 6.5 Recap: Selective Operations · Easy
```bash
mkdir archive
mv photo_[0-9].jpg archive/
mv report_?.txt archive/
rm *.png
ls
ls archive
```

---

## 7. File Content

### 7.1 Head And Tail · Beginner
```bash
head -n 3 log.txt; tail -n 1 log.txt
```

### 7.2 Word Count · Beginner
```bash
wc -l notes.txt
wc -w notes.txt
```

### 7.3 Sort Command · Beginner
```bash
sort fruits.txt
sort -r fruits.txt
```

### 7.4 Unique Command · Beginner
```bash
sort duplicates.txt | uniq
```
`uniq` only removes **adjacent** duplicates, so sort first.

### 7.5 Grep Basics · Beginner
```bash
grep "error" server.log
```

### 7.6 Grep With Flags · Easy
```bash
grep -n "error" server.log    # with line numbers
grep -c "info" server.log     # count matching lines
grep -v "info" server.log     # invert: lines WITHOUT info
```

### 7.7 Recap: Text Detective · Easy
```bash
head -n 3 server.log
wc -l server.log
sort fruits.txt
sort duplicates.txt | uniq
grep -n "error" server.log
```

---

## 8. Redirection

### 8.1 Standard Output · Easy
```bash
echo "Terminal streams are powerful"
```

### 8.2 Overwrite To A File · Easy
```bash
echo "System initialized" > status.txt
cat status.txt
```

### 8.3 Append To A File · Easy
```bash
echo "Call the dentist" >> tasks.txt
cat tasks.txt
```
`>` overwrites the file. `>>` appends to it.

### 8.4 Standard Input · Easy
```bash
sort -n < numbers.txt
```

### 8.5 Standard Error · Easy
```bash
ls missing_folder 2> errors.txt
cat errors.txt
```
File descriptor 2 is stderr, so `2>` captures error messages separately from normal output.

### 8.6 Recap: Log Builder · Easy
```bash
echo "Session started" > activity.log
echo "User logged in" >> activity.log
echo "Task completed" >> activity.log
ls invalid_path 2>> activity.log
cat activity.log
```

---

## 9. Piping

### 9.1 What Is A Pipe · Easy
```bash
cat fruits.txt | sort
```

### 9.2 Chaining Two Commands · Easy
```bash
cat messages.txt | head -4
```

### 9.3 Chaining Multiple Commands · Easy
**Task:** Count the unique visitors.
```bash
cat visitors.txt | sort | uniq | wc -l
```

### 9.4 Pipe With Grep · Easy
**Task:** Count the HTTP 500 responses in the access log.
```bash
cat access.log | grep "500" | wc -l
```

### 9.5 Recap: Data Pipeline · Easy
**Task:** Count the unique electronics products sold.
```bash
cat sales.txt | grep "electronics" | sort | uniq | wc -l
```

---

## 10. Log Analyzer Project

Analyze a 10-line application log, then produce a shareable report.

### 10.1 Project Overview · Easy
```bash
cat server.log
```

### 10.2 Viewing The Log File · Easy
```bash
head -n 5 server.log
```

### 10.3 Filtering Errors · Easy
```bash
grep "ERROR" server.log
```

### 10.4 Counting Occurrences · Easy
```bash
grep "ERROR" server.log | wc -l     # 3
```

### 10.5 Generating A Report · Easy
```bash
echo "=== Log Analysis Report ===" > report.txt
echo "Total errors found: $(grep ERROR server.log | wc -l)" >> report.txt
echo "=== Error Details ===" >> report.txt
grep ERROR server.log >> report.txt
cat report.txt
```
`$(…)` (command substitution) puts a command's output inside a string. This is the same filter, count and report pattern used in SOC log triage.

---

## 11. Permissions

### 11.1 Understanding Permissions · Easy
```bash
ls -l
```
The first character is the file type (`-` file, `d` directory). It's followed by rwx for the owner, group and others, then the owner and group names.

### 11.2 Reading Permissions · Easy
```bash
ls -l
```
Directories need `x` so users can enter them.

### 11.3 Chmod With Numbers · Easy
```bash
chmod 744 deploy.sh
ls -l deploy.sh      # -rwxr--r--
```
Read = 4, write = 2, execute = 1, so 7 = rwx and 4 = r--.

### 11.4 Chmod With Symbols · Easy
```bash
chmod u+x setup.sh
ls -l setup.sh       # -rwx------
```

### 11.5 File Ownership · Concept lesson (no graded challenge)
```bash
ls -l report.txt            # owner and group appear after the permission string
# sudo chown alice:developers report.txt   # change owner:group
```

### 11.6 Recap: Lock It Down · Easy
```bash
chmod 754 backup.sh      # -rwxr-xr--
chmod 600 config.txt     # -rw-------
ls -l backup.sh config.txt
```
`600` on secrets and config files is least privilege in practice.

---

## 12. Environment

### 12.1 Environment Variables · Easy
```bash
echo $SHELL
```

### 12.2 View All Variables · Easy
```bash
printenv USER
```

### 12.3 The PATH Variable · Easy
```bash
which cat
```
`which` searches the directories listed in `$PATH`.

### 12.4 Setting A Variable · Easy
```bash
GREETING="Hello, Terminal!"
echo $GREETING
```
There must be no spaces around `=`.

### 12.5 Export A Variable · Easy
```bash
export PROJECT="terminal-course"
env | grep PROJECT
```
`export` makes a variable visible to child processes.

### 12.6 The Profile File · Easy
```bash
echo 'export WORKSPACE="/home/user/projects"' >> ~/.bashrc
source ~/.bashrc
echo $WORKSPACE
```
The single quotes write the line literally. `source` reloads the file in the current shell.

### 12.7 Recap: Custom Environment · Easy
```bash
export APP_NAME="MyTerminalApp"
export APP_VERSION="1.0"
echo "$APP_NAME v$APP_VERSION"
```

---

## 13. Shell Scripting Basics

### 13.1 What Is A Shell Script · Easy
```bash
cat hello.sh
```
The `#!/bin/bash` shebang tells the system which interpreter runs the script.

### 13.2 Create And Run A Script · Easy
```bash
echo '#!/bin/bash' > welcome.sh
echo 'echo "Welcome to scripting!"' >> welcome.sh
chmod +x welcome.sh
./welcome.sh
```

### 13.3 Variables In Scripts · Easy
```bash
cat > intro.sh << 'EOF'
#!/bin/bash
language="Bash"
skill="scripting"
echo "Learning $language $skill!"
EOF
chmod +x intro.sh
./intro.sh
```
The quoted `'EOF'` keeps `$language` literal while the file is being written.

### 13.4 User Input In Scripts · Easy
```bash
cat > greeter.sh << 'EOF'
#!/bin/bash
read -p "What is your favorite color? " color
echo "Nice! $color is a great color!"
EOF
chmod +x greeter.sh
echo "blue" | ./greeter.sh
```
The terminal isn't interactive, so the answer is piped into `read`.

### 13.5 If Statement · Easy
```bash
cat > check_score.sh << 'EOF'
#!/bin/bash
score=75
if [ $score -ge 70 ]; then
  echo "You passed!"
else
  echo "You failed."
fi
EOF
chmod +x check_score.sh
./check_score.sh
```

### 13.6 For Loop · Easy
```bash
cat > countdown.sh << 'EOF'
#!/bin/bash
for i in {5..1}; do
  echo "Countdown: $i"
done
EOF
chmod +x countdown.sh
./countdown.sh
```

### 13.7 While Loop · Easy
```bash
cat > counter.sh << 'EOF'
#!/bin/bash
num=1
while [ $num -le 4 ]; do
  echo "Number: $num"
  num=$((num + 1))
done
EOF
chmod +x counter.sh
./counter.sh
```

### 13.8 Recap: Number Guesser · Easy
```bash
cat > guesser.sh << 'EOF'
#!/bin/bash
secret=5
guess=0
while [ $guess -ne $secret ]; do
  guess=$((guess + 1))
  if [ $guess -lt $secret ]; then
    echo "Guess $guess: Too low!"
  elif [ $guess -eq $secret ]; then
    echo "Guess $guess: Correct!"
  fi
done
EOF
chmod +x guesser.sh
./guesser.sh
```

---

## 14. Backup Script Project

Build `backup.sh` step by step: prompt for a folder, create a timestamped backup folder, copy the files, and print a summary.

### 14.1 Project Overview · Easy
```bash
ls documents
```

### 14.2 Getting The Source Path · Easy
```bash
cat > backup.sh << 'EOF'
#!/bin/bash
read -p "Enter source directory: " source_dir
echo "Backing up: $source_dir"
EOF
chmod +x backup.sh
echo "documents" | ./backup.sh
```

### 14.3 Creating Backup Folder · Easy
```bash
cat > backup.sh << 'EOF'
#!/bin/bash
read -p "Enter source directory: " source_dir
echo "Backing up: $source_dir"
backup_dir="backup"
mkdir -p "$backup_dir"
echo "Created backup folder: $backup_dir"
EOF
chmod +x backup.sh
echo "documents" | ./backup.sh
ls
```

### 14.4 Copying The Files · Easy
```bash
echo 'cp "$source_dir"/* "$backup_dir"' >> backup.sh
echo 'echo "Files copied successfully"' >> backup.sh
echo "documents" | bash backup.sh
ls backup
```

### 14.5 Adding A Timestamp · Easy
```bash
cat > backup.sh << 'EOF'
#!/bin/bash
read -p "Enter source directory: " source_dir
echo "Backing up: $source_dir"
timestamp=$(date +%Y%m%d_%H%M%S)
backup_dir="backup_$timestamp"
mkdir -p "$backup_dir"
echo "Created backup folder: $backup_dir"
cp "$source_dir"/* "$backup_dir"
echo "Files copied successfully"
EOF
echo "documents" | bash backup.sh
```

### 14.6 Success Message (final script) · Easy
```bash
cat > backup.sh << 'EOF'
#!/bin/bash
read -p "Enter directory to backup: " source_dir
echo "Backing up: $source_dir"
backup_dir="backup_$(date +%Y%m%d_%H%M%S)"
mkdir "$backup_dir"
echo "Created backup folder: $backup_dir"
cp "$source_dir"/* "$backup_dir"/
echo "Files copied successfully"
echo "Backup complete: $source_dir -> $backup_dir"
EOF
echo "documents" | bash backup.sh
```
Sample output:
```
Backing up: documents
Created backup folder: backup_20240115_143052
Files copied successfully
Backup complete: documents -> backup_20240115_143052
```

---

## 15. Final Challenges

### 15.1 File Organizer By Type · Easy
**Task:** Sort the files in `downloads/` into subfolders by extension.
```bash
cat > organize.sh << 'EOF'
#!/bin/bash
for file in downloads/*; do
  [ -f "$file" ] || continue
  name=$(basename "$file")
  ext="${file##*.}"
  mkdir -p "downloads/$ext"
  mv "$file" "downloads/$ext/"
  echo "Moved $name to $ext/"
done
EOF
chmod +x organize.sh
./organize.sh
ls downloads        # jpg  mp3  pdf  txt
```
`${file##*.}` strips everything up to the last `.`, which leaves the extension.

### 15.2 Word Frequency Counter · Easy
**Task:** Rank the words in `article.txt` by how often they appear.
```bash
cat article.txt | tr ' ' '\n' | sort | uniq -c | sort -rn
```
Top results: `7 the`, `5 fox`, `3 quick`, `3 dog`. The "count and rank" pattern is used constantly for log analysis (top IPs, top errors).

### 15.3 System Health Checker · Easy
**Task:** Print a small system report using command substitution.
```bash
cat > health.sh << 'EOF'
#!/bin/bash
echo "=== System Health Report ==="
echo "User: $(id -u)"
echo "Current Directory: $(pwd)"
echo "Running Processes: $(wc -l < processes.txt)"
echo "=== End of Report ==="
EOF
chmod +x health.sh
./health.sh
```

---

## Command cheat sheet

| Area | Commands |
| --- | --- |
| Navigate | `pwd` · `ls [-a] [-l]` · `cd DIR` · `cd /` · `cd` |
| Files | `touch` · `cat` · `cp` · `mv` · `rm` |
| Directories | `mkdir [-p]` · `cp -r` · `mv` · `rm -r` |
| Wildcards | `*` (any) · `?` (one char) · `[1-3]` (range) |
| Text | `head -n` · `tail -n` · `wc -l/-w` · `sort [-r/-n/-rn]` · `uniq [-c]` · `grep [-n/-c/-v/-i]` · `tr` |
| Redirection | `>` overwrite · `>>` append · `<` stdin · `2>` / `2>>` stderr · `|` pipe |
| Permissions | `ls -l` · `chmod 754` · `chmod u+x` · `chown` |
| Environment | `echo $VAR` · `printenv` · `export` · `which` · `source ~/.bashrc` |
| Scripting | `#!/bin/bash` · `read -p` · `if [ ]; then; fi` · `for i in {5..1}` · `while [ ]; do; done` · `$(cmd)` · `$((x + 1))` |
