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
