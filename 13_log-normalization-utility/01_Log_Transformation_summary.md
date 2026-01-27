# Log Transformation Technical Summary
Working Directory: `/home/analyst/lab.support.files/`
Target File: `applicationX_in_epoch.log`
## 1. Command Breakdown
`cd /home/analyst/lab.support.files/`
Purpose: Changes the current working directory to the location of the log files.
`ls -l`
Purpose: Lists files in long format to verify permissions, size, and existence before processing.
```bash
awk 'BEGIN {FS=OFS="|"} {$3=strftime("%c",$3)} {print}' applicationX_in_epoch.log
```
`awk:` Invokes the pattern scanning and processing language.
`BEGIN {FS=OFS="|"}:` Sets both the input Field Separator and Output Field Separator to the pipe character `(|).`
`$3=strftime("%c",$3):` Targets the 3rd column (the Epoch timestamp) and transforms it into a human-readable date format using the system's locale.
`{print}:` Outputs the modified line to the terminal.
## 2. Log Cleanup & Normalization
### Issue Identified:
The original log contained a trailing empty line. When processed by AWK, this null value was interpreted as 0, resulting in the Unix Epoch start date: Wed 31 Dec 1969.
### Resolution:
`nano applicationX_in_epoch.log:` Opens the file in a text editor to manually remove the trailing newline.
Verification: Running the AWK command again ensures the 1969 artifact is removed.
## 3. Data Persistence
```bash
awk '...' applicationX_in_epoch.log > applicationX_in_human.log
```
### Operation: Output Redirection.
Result: Instead of printing to the terminal (STDOUT), the processed data is saved into a new file named applicationX_in_human.log.
`cat applicationX_in_human.log`
Purpose: Displays the contents of the newly created file to verify successful transformation and formatting.
