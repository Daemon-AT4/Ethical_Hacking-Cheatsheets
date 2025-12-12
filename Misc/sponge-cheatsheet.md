# 🧽 Sponge Command Cheat Sheet

## 📖 Introduction

`sponge` is a command-line utility from the **moreutils** package that reads all input from stdin before writing to a file. Unlike standard shell redirects (`>`), which open the output file immediately (potentially truncating it before reading completes), `sponge` buffers the entire input in memory first, then writes it out.

This "read-all-then-write" behavior makes `sponge` essential for **safely modifying files in-place** within pipelines.

### 🔧 Installation

```bash
# Debian/Ubuntu
sudo apt install moreutils

# Fedora/RHEL
sudo dnf install moreutils

# Arch Linux
sudo pacman -S moreutils

# macOS (Homebrew)
brew install moreutils
```

### ⚙️ Basic Syntax

```bash
command | sponge [OPTIONS] filename
```

### 🎛️ Options

| Option | Description |
|--------|-------------|
| `-a` | Append to file instead of overwriting |
| No options | Overwrite the file with buffered content |

---

## ⚠️ The Problem Sponge Solves

Without `sponge`, this command **destroys your data**:

```bash
# ❌ DANGEROUS - file gets truncated before reading completes
grep 'pattern' file.txt > file.txt
# Result: empty file!
```

The shell opens `file.txt` for writing (truncating it) before `grep` starts reading.

With `sponge`, the operation is **safe**:

```bash
# ✅ SAFE - sponge buffers all input before writing
grep 'pattern' file.txt | sponge file.txt
```

---

## 🔍 Using Sponge with grep

`grep` searches for patterns in files. Combined with `sponge`, you can filter files in-place.

### Keep Only Matching Lines

```bash
# Keep only lines containing 'error'
grep 'error' logfile.txt | sponge logfile.txt
```

### Remove Matching Lines

```bash
# Remove all lines containing 'DEBUG'
grep -v 'DEBUG' application.log | sponge application.log
```

### Remove Comments from Config Files

```bash
# Remove lines starting with # (comments)
grep -v '^#' config.conf | sponge config.conf
```

### Remove Empty Lines

```bash
# Remove blank lines from a file
grep -v '^$' data.txt | sponge data.txt
```

### Case-Insensitive Filtering

```bash
# Keep lines containing 'warning' (case-insensitive)
grep -i 'warning' alerts.log | sponge alerts.log
```

### Chain Multiple grep Commands

```bash
# Remove comments AND empty lines
grep -v '^#' config.conf | grep -v '^$' | sponge config.conf
```

---

## ✏️ Using Sponge with sed

`sed` (stream editor) performs text transformations. With `sponge`, you can apply complex edits in-place safely.

### Simple Find and Replace

```bash
# Replace 'old' with 'new' (first occurrence per line)
sed 's/old/new/' file.txt | sponge file.txt
```

### Global Replacement

```bash
# Replace ALL occurrences of 'foo' with 'bar'
sed 's/foo/bar/g' file.txt | sponge file.txt
```

### Case-Insensitive Replacement

```bash
# Replace 'error' regardless of case
sed 's/error/warning/gi' logfile.txt | sponge logfile.txt
```

### Delete Specific Lines

```bash
# Delete line 5
sed '5d' file.txt | sponge file.txt

# Delete lines 10 through 20
sed '10,20d' file.txt | sponge file.txt

# Delete the last line
sed '$d' file.txt | sponge file.txt
```

### Delete Lines Matching Pattern

```bash
# Delete all lines containing 'obsolete'
sed '/obsolete/d' file.txt | sponge file.txt
```

### Remove Leading/Trailing Whitespace

```bash
# Remove leading whitespace
sed 's/^[[:space:]]*//' file.txt | sponge file.txt

# Remove trailing whitespace
sed 's/[[:space:]]*$//' file.txt | sponge file.txt

# Remove both
sed 's/^[[:space:]]*//;s/[[:space:]]*$//' file.txt | sponge file.txt
```

### Convert Tabs to Spaces

```bash
# Replace tabs with 4 spaces
sed 's/\t/    /g' code.py | sponge code.py
```

### Multiple Substitutions

```bash
# Chain multiple replacements
sed -e 's/apple/orange/g' -e 's/banana/grape/g' fruits.txt | sponge fruits.txt
```

### Remove Carriage Returns (Windows → Unix)

```bash
# Convert Windows line endings to Unix
sed 's/\r$//' file.txt | sponge file.txt
```

---

## ✂️ Using Sponge with cut

`cut` extracts sections from each line of input. With `sponge`, you can trim files down to specific columns.

### Extract Specific Field (Delimiter-Based)

```bash
# Extract usernames (first field) from /etc/passwd format
cut -d':' -f1 users.txt | sponge users.txt
```

### Extract Multiple Fields

```bash
# Keep fields 1 and 3 (colon-delimited)
cut -d':' -f1,3 data.txt | sponge data.txt
```

### Extract Range of Fields

```bash
# Keep fields 2 through 5
cut -d',' -f2-5 data.csv | sponge data.csv
```

### Extract by Character Position

```bash
# Keep only first 20 characters of each line
cut -c1-20 file.txt | sponge file.txt
```

### Extract with Tab Delimiter (Default)

```bash
# Extract second column (tab-separated)
cut -f2 data.tsv | sponge data.tsv
```

### Remove a Specific Column

```bash
# Keep all fields EXCEPT field 3
cut -d',' -f1,2,4- data.csv | sponge data.csv
```

### Extract from Specific Position to End

```bash
# Keep characters 10 to end of each line
cut -c10- file.txt | sponge file.txt
```

---

## 📋 Using Sponge with paste

`paste` merges lines from multiple files side by side. With `sponge`, you can combine and overwrite efficiently.

### Merge Two Files Side by Side

```bash
# Combine file1 and file2 with tabs between them
paste file1.txt file2.txt | sponge combined.txt
```

### Merge with Custom Delimiter

```bash
# Combine files with comma separator
paste -d',' file1.txt file2.txt | sponge combined.csv
```

### Convert Column to Row (Serial Mode)

```bash
# Join all lines into a single line with commas
paste -sd',' file.txt | sponge file.txt
```

### Create Tab-Separated Output

```bash
# Merge three files with tabs
paste file1.txt file2.txt file3.txt | sponge merged.tsv
```

### Transpose Single File (Column → Row)

```bash
# Convert newline-separated values to comma-separated
cat values.txt | paste -sd',' | sponge values.txt
# Input:  apple    Output: apple,banana,cherry
#         banana
#         cherry
```

### Merge with Multiple Delimiters

```bash
# Alternate between colon and newline
paste -d':\n' file1.txt file2.txt | sponge result.txt
```

### Combine paste with cut

```bash
# Extract field 1 from both files and merge
paste <(cut -d',' -f1 a.csv) <(cut -d',' -f1 b.csv) | sponge ids.txt
```

---

## 🔢 Using Sponge with sort

`sort` orders lines alphabetically or numerically. With `sponge`, you can sort files in-place.

### Basic Alphabetical Sort

```bash
# Sort lines alphabetically
sort file.txt | sponge file.txt
```

### Reverse Sort

```bash
# Sort in descending order
sort -r file.txt | sponge file.txt
```

### Numeric Sort

```bash
# Sort numerically (not lexicographically)
sort -n numbers.txt | sponge numbers.txt
```

### Sort and Remove Duplicates

```bash
# Sort and keep only unique lines
sort -u file.txt | sponge file.txt
```

### Sort by Specific Column

```bash
# Sort by second column (comma-delimited)
sort -t',' -k2 data.csv | sponge data.csv

# Sort by third column numerically
sort -t',' -k3n data.csv | sponge data.csv
```

### Case-Insensitive Sort

```bash
# Ignore case when sorting
sort -f names.txt | sponge names.txt
```

### Human-Readable Numeric Sort

```bash
# Sort file sizes (1K, 2M, 3G, etc.)
sort -h sizes.txt | sponge sizes.txt
```

### Sort IP Addresses

```bash
# Sort IP addresses correctly
sort -t'.' -k1,1n -k2,2n -k3,3n -k4,4n ips.txt | sponge ips.txt
```

### Stable Sort

```bash
# Preserve original order for equal elements
sort -s -k2 data.txt | sponge data.txt
```

---

## 🔗 Combining Multiple Tools

The real power comes from chaining tools together.

### Clean and Sort Config File

```bash
# Remove comments, empty lines, and sort
grep -v '^#' config.conf | grep -v '^$' | sort -u | sponge config.conf
```

### Extract, Transform, and Sort

```bash
# Get usernames, make lowercase, sort uniquely
cut -d':' -f1 passwd.txt | sed 's/.*/\L&/' | sort -u | sponge users.txt
```

### Filter and Format Log Entries

```bash
# Extract error lines, keep timestamp and message, sort by time
grep 'ERROR' app.log | cut -d' ' -f1,4- | sort | sponge errors.txt
```

### Deduplicate and Clean Data

```bash
# Remove duplicates, trim whitespace, sort
sort data.txt | uniq | sed 's/^[[:space:]]*//;s/[[:space:]]*$//' | sponge data.txt
```

### CSV Processing Pipeline

```bash
# Remove header, sort by column 2, add header back
tail -n +2 data.csv | sort -t',' -k2 | (head -1 data.csv && cat) | sponge data.csv
```

---

## 📎 Appending with Sponge

Use the `-a` flag to append instead of overwrite.

```bash
# Append filtered results to existing file
grep 'WARN' new_logs.txt | sponge -a all_warnings.txt
```

---

## 💡 Pro Tips

1. **Memory Considerations**: `sponge` buffers all input in memory. For very large files (larger than available RAM), consider alternatives or ensure sufficient memory.

2. **Atomic Operations**: When possible, `sponge` updates files atomically by writing to a temp file first, then renaming.

3. **Preserve Permissions**: `sponge` attempts to preserve file permissions when overwriting.

4. **Test First**: Always test your pipeline without `sponge` first to verify output:
   ```bash
   # Test output first
   grep -v 'pattern' file.txt | head
   
   # Then apply with sponge
   grep -v 'pattern' file.txt | sponge file.txt
   ```

5. **Backup Important Files**: For critical files, make a backup first:
   ```bash
   cp important.txt important.txt.bak
   sed 's/old/new/g' important.txt | sponge important.txt
   ```

---

## 📊 Quick Reference Table

| Tool | Common Use with Sponge | Example |
|------|------------------------|---------|
| `grep` | Filter lines in-place | `grep 'keep' f.txt \| sponge f.txt` |
| `grep -v` | Remove lines in-place | `grep -v 'remove' f.txt \| sponge f.txt` |
| `sed` | Find/replace in-place | `sed 's/a/b/g' f.txt \| sponge f.txt` |
| `cut` | Extract columns in-place | `cut -d',' -f1 f.csv \| sponge f.csv` |
| `paste` | Merge files | `paste a.txt b.txt \| sponge c.txt` |
| `sort` | Sort in-place | `sort f.txt \| sponge f.txt` |
| `sort -u` | Sort + dedupe in-place | `sort -u f.txt \| sponge f.txt` |

---

## 🔗 See Also

- `tee` - Write to file while also passing to stdout (but not safe for in-place)
- `moreutils` - The package containing sponge and other useful utilities
- `sed -i` - Built-in in-place editing (but requires temp file internally)
- `sort -o` - Sort's built-in in-place output option

---

*Created for efficient command-line text processing workflows* 🐧
