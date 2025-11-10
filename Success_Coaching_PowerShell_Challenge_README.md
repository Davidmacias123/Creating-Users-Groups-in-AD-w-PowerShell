# Success Coaching Challenge
## Introduction to PowerShell & PowerShell ISE

##  Purpose
A simple, hands-on walkthrough for PowerShell basics that real system helpers use every day:
- see running programs  
- move through directories  
- create and read files  
- check if a file changed (integrity)

Every command and on-screen column is explained in simple, everyday words.

---

##  Learning Objectives
1. Navigate directories and list contents.  
2. Observe running processes and understand each column.  
3. Create directories and files; write and read text.  
4. Verify file integrity using file hashes.  
5. Practice all steps inside PowerShell ISE.

---

##  Scenario
A small “system health and integrity” report is needed:
- list processes,  
- show directory items,  
- create files and record notes,  
- confirm file integrity before and after a change.

---

#  Challenge Steps (Original vs. Used Commands + Deep Explanations)

---

## 1) Processes — `Get-Process`, `Sort-Object`, `Select-Object`, `Where-Object`

### A) Show all running processes

**Original command (base):**
```powershell
Get-Process
```

**Used in this challenge (same as base):**
```powershell
Get-Process
```

**Plain meaning:** show every program and background task currently running.

**What the common columns mean (plain words):**
- **ProcessName** — program/service name (friendly label).
- **Id** — unique number for that running program. Helpful when targeting or stopping a specific process.
- **CPU** — total time on the processor (in seconds). Bigger number = has been busy for longer.
- **Handles** — small “doors” into Windows resources (files, registry keys, windows). More handles = touching more system pieces.
- **WS** (Working Set) — memory in use right now (RAM), shown in KB. Can grow/shrink as the app works or rests.
- **PM** (Paged Memory Size) — memory that Windows can move out to disk if needed.
- **NPM** (Non-Paged Memory Size) — memory that must stay in RAM; if very large, system pressure can rise.
- **SI** (Session Id) — which login/session the process belongs to (0 often means system session).
- **StartTime** — when the process started. Helps spot old or stuck processes. (Might be blank without enough rights.)
- **Responding** — for apps with windows: True = reacting, False = frozen.
- **Path** — full file path to the program’s EXE. (May need admin to see.)

<img width="1023" height="767" alt="Get-Process command" src="https://github.com/user-attachments/assets/622ec23d-a435-4ded-99c1-d4395a2905d5" />



<img width="1012" height="757" alt="getprocess svchost" src="https://github.com/user-attachments/assets/51533959-07d0-4ea9-a3e7-a93da2850ec5" />


<img width="1022" height="716" alt="getprocess extra work" src="https://github.com/user-attachments/assets/6ea18bc9-1c1f-448a-ae65-6b1681b4ec0c" />

---

### B) Show the 10 most CPU-hungry processes

**Original command (base form):**
```powershell
Sort-Object <Property> [-Descending|-Ascending]
Select-Object -First <N>
```

**Used in this challenge:**
```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
```

**Plain meaning:** sort by CPU from biggest to smallest, then keep only the first 10 rows.

**Every part:**
- **`|` (pipe)** — “and then”; send results to the next command.  
- **Sort-Object** — put rows in order.  
- **CPU** — the property used for sorting.  
- **-Descending** — biggest to smallest (top busy first).  
- **Select-Object** — pick part of the list.  
- **-First 10** — keep only the first 10 rows.

---

### C) Show only “busy” processes (CPU over a number)

**Original command (base form):**
```powershell
Where-Object { <property> <operator> <value> }
```

**Used in this challenge:**
```powershell
Get-Process | Where-Object { $_.CPU -gt 50 }
```

**Plain meaning:** keep only rows where CPU is greater than 50.

**Every part:**
- **Where-Object** — filter rows by a rule.  
- **{ … }** — the rule lives inside.  
- **$_** — “the current row” moving through the pipe.  
- **.CPU** — read the CPU number from that row.  
- **-gt** — “greater than.”  
- **50** — the limit used for the rule.

---

### D) Show a specific process name

**Original command (base form):**
```powershell
Where-Object { $_.<Property> -eq "<Text>" }
```

**Used in this challenge:**
```powershell
Get-Process | Where-Object { $_.ProcessName -eq "svchost" }
```

**Plain meaning:** keep only rows where the name equals “svchost.”

**Every part:**
- **ProcessName** — the visible name of the program.  
- **-eq** — equals.  
- **"svchost"** — target name.

---

## 2) Directories — `Get-ChildItem`, `Set-Location`, `cd ..`

### A) List items in the current directory

**Original command (base):**
```powershell
Get-ChildItem
```

**Used in this challenge (same as base):**
```powershell
Get-ChildItem
```

**Plain meaning:** show files and sub-directories in the current place.

**What the columns mean:**
- **Mode** — quick type/attribute flags.  
  - First spot: `d` = directory, `-` = file  
  - Other spots: `a` = archive, `r` = read-only, `h` = hidden, `s` = system  
- **LastWriteTime** — last time the item changed (saved).  
- **Length** — file size in bytes (directories show blank or 0).  
- **Name** — item name (file or directory).


<img width="1023" height="767" alt="get childitem part 1" src="https://github.com/user-attachments/assets/d5554d07-f625-4d3a-be06-020501613edd" />


<img width="1022" height="763" alt="get childitem part 2" src="https://github.com/user-attachments/assets/d7334b06-5e83-4949-85b2-f4d167cd960a" />

---

### B) Move into a directory

**Original command (base):**
```powershell
Set-Location <PathOrName>
```

**Used in this challenge:**
```powershell
Set-Location Downloads
```

**Plain meaning:** change current place to the Downloads directory.

---

### C) Go up one directory

**Original command (base):**
```powershell
cd ..
```

**Used in this challenge (same as base):**
```powershell
cd ..
```

**Plain meaning:** move to the parent directory (one level up).

---

## 3) Create / Write / Read — `New-Item`, `Add-Content`, `Get-Content`

### A) Create a directory
```powershell
New-Item -Path "C:\Users\Administrator\Funnystuff" -ItemType Directory
```

**Meaning:** create a directory named Funnystuff.

**Every part:**
- **New-Item** — create something new.  
- **-Path** — where to create it.  
- **-ItemType** — tells the kind of thing to make.  
- **Directory** — the kind of thing is a directory.


<img width="1025" height="472" alt="newitemtype" src="https://github.com/user-attachments/assets/1d42c3d2-876b-4769-bb56-708ebea30365" />

---

### B) Create a file
```powershell
New-Item -Path "C:\Users\Administrator\Funnystuff\Cheetos" -ItemType File
```

**Meaning:** create an empty file named Cheetos inside Funnystuff.

**Every part:**
- **New-Item** — create new item.  
- **-Path** — full path and name for the new file.  
- **-ItemType** — what kind to create.  
- **File** — a file (not a directory).

---

### C) Add text to a file
```powershell
Add-Content -Path "C:\Users\Administrator\Funnystuff\Cheetos" -Value "Hot cheetos are better than regular cheetos"
```

**Meaning:** write that sentence at the end of the file.

**Every part:**
- **Add-Content** — append text.  
- **-Path** — which file to modify.  
- **-Value** — what text to add.

---

### D) Read file content
```powershell
Get-Content -Path "C:\Users\Administrator\Funnystuff\Cheetos"
```

**Meaning:** show the lines inside the file.


<img width="1106" height="822" alt="creating two newitemtype with content" src="https://github.com/user-attachments/assets/4e1959f3-3ae8-45db-a527-9c4b810d809b" />

---

## 4) File Integrity — `Get-FileHash`

### A) Make a fingerprint (hash)
```powershell
Get-FileHash -Path "C:\Users\Administrator\Funnystuff\Fries.txt"
```

**Meaning:** create a digital fingerprint of the file contents.

**Every part:**
- **Get-FileHash** — compute a hash for the file.  
- **-Path** — which file to scan.  
- **-Algorithm** (optional) — pick the math used (default SHA256).

**How to read the output:**
- **Algorithm** — which math was used (e.g., SHA256).  
- **Hash** — the fingerprint itself (a long hex string).  
- **Path** — the file that was scanned.


<img width="1026" height="127" alt="getfilehashh" src="https://github.com/user-attachments/assets/6714bc5e-bd61-499e-a9bb-843e072bdff7" />

---

### B) Change the file (append more text)
```powershell
Add-Content -Path "C:\Users\Administrator\Funnystuff\Fries.txt" -Value "Garlic is also a good sauce for fries."
```

**Meaning:** add another line; content is now different.

---

### C) Make a fingerprint again
```powershell
Get-FileHash -Path "C:\Users\Administrator\Funnystuff\Fries.txt"
```

**Meaning:** compute a new hash. If it changed, the file was modified.


<img width="1022" height="761" alt="getfilehashh part2" src="https://github.com/user-attachments/assets/50801ee8-a046-4dce-a2a9-75c72b39d792" />

---

#  Operators and Symbols (Plain Words)
- **`|` (pipe)** — “and then”; send results from left to right.  
- **`{}`** — rule or mini-script.  
- **`$_`** — the current row in the pipeline.  
- **`.`** — look at a property.  
- **`-eq`** — equals.  
- **`-gt`** — greater than.  
- **`-lt`** — less than.  
- **`-like`** — match text pattern.  
- **`-First N`** — keep first N results.  
- **`-Descending` / `-Ascending`** — sort order.  
- **`..`** — parent directory.

---

##  Full command list covered
- Get-Process  
- Sort-Object  
- Select-Object  
- Where-Object  
- Get-ChildItem  
- Set-Location  
- cd ..  
- New-Item  
- Add-Content  
- Get-Content  
- Get-FileHash

---

## 🏁 Summary
All commands from the challenge are included with both original and used forms.  
Every on-screen column and every parameter is explained in plain language.  
File integrity is shown with hash changes before and after editing.

---

**What This Project Is All About**

This GitHub project is part of a Success Coaching Challenge focused on learning and practicing PowerShell — a tool that helps control and manage computers by typing commands instead of clicking buttons.

The main goal of this challenge is to learn how to think like a system administrator by performing real tasks: checking what programs are running, moving around directories, creating files, writing text inside them, and checking if those files have changed.

Throughout this project, every step was done inside PowerShell ISE, a place where commands can be written, tested, and saved as scripts.

---

**What Was Done in This Challenge**

Get-Process was used to look at all the programs and background tasks running on the computer. It also showed details like CPU usage, memory, process ID, and more.

Sort-Object and Select-Object were used to organize and display the top programs using the most CPU power.

Where-Object helped filter specific programs, like showing only “svchost” or those using a lot of CPU.

Get-ChildItem and Set-Location were used to explore different directories on the computer. This made it possible to see files and move into other locations.

New-Item was used to create a brand-new directory and file.

Add-Content was used to write words inside that file.

Get-Content was used to read the text that was written.

Get-FileHash was used to check if the file changed after adding new text — this step proved how PowerShell can help check data integrity.

---

**What This Project Teaches**

How to see what a computer is doing behind the scenes.

How to move between directories and understand file structures.

How to create and manage files with simple commands.

How to check if files were changed by comparing their digital fingerprints (hashes).

How to read and understand columns and parameters shown by PowerShell commands, like CPU, ID, Handles, Mode, and Length.

---

**Why This Project Matters**

This project builds the foundation of what real system administrators and IT professionals do daily. Every command practiced here helps build real-world confidence in using PowerShell.

By completing this challenge, it becomes clear how simple commands can help manage files, monitor processes, and keep systems secure. Even for someone brand new to IT, this project makes PowerShell feel simple and fun to learn.
