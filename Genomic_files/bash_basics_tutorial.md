Introduction to Bash
================
21-02-2026

<style>
  body { 
    word-spacing: 5px;
    line-height: 1.5;
    font-size: 16px;
    text-align: justify;
  }
</style>

## A Practical Guide for Beginners in Genomics

------------------------------------------------------------------------

## 1. What is Bash?

Bash stands for **Bourne Again SHell**. It is a command-line interpreter
— a program that reads text commands you type and tells your computer to
execute them. Instead of clicking through folders and menus (a Graphical
User Interface, or GUI), you interact with the computer through a
text-based interface called the **terminal** or **shell**. This might
feel unfamiliar at first, but it quickly becomes one of the most
powerful tools in a bioinformatician’s toolkit.

Why use Bash instead of clicking around? Because with Bash you can
automate repetitive tasks, work on remote servers (like high-performance
computing clusters), handle files that are far too large to open in any
regular program, and reproduce your analyses exactly. In genomics, where
you might work with thousands of files or sequence reads numbering in
the billions, these advantages are not optional — they are essential.

------------------------------------------------------------------------

## 2. Mac vs. Windows (MobaXterm): Key Differences

This course is taught on a Mac, but most of you will be using
**MobaXterm** on Windows. MobaXterm is a software application for
Windows that gives you a Unix-like terminal, making it possible to run
Bash commands just like on a Mac or Linux system. For most Bash commands
covered in this course, everything will work identically. However, there
are a few important platform-specific differences to be aware of.

### Keyboard Shortcuts

The most common source of confusion is the difference between the
modifier keys used for shortcuts.

| Action                      | Mac        | Windows (MobaXterm)       |
|-----------------------------|------------|---------------------------|
| Copy text                   | `Cmd + C`  | `Ctrl + C` ‼️             |
| Paste text                  | `Cmd + V`  | `Ctrl + V` or right-click |
| Open new terminal tab       | `Cmd + T`  | Varies                    |
| Interrupt a running command | `Ctrl + C` | `Ctrl + C`                |

> ‼️ **Critical warning for Windows users:** In most Windows
> applications, `Ctrl + C` copies text. In the terminal, `Ctrl + C`
> **interrupts (stops) a running command**. This is a universal terminal
> behaviour. To copy text in MobaXterm, you can usually **highlight it
> with your mouse** and it copies automatically, or right-click and
> select Copy.

### Copying a File Path

On a **Mac**, you can right-click a file in Finder, hold the `Option`
key, and select “Copy \[filename\] as Pathname” to get its full path.

On **Windows**, you can right-click a file in File Explorer, hold
`Shift`, and select “Copy as path.” However, Windows uses
**backslashes** (`\`) to separate folders (e.g.,
`C:\Users\Name\Downloads`), while Bash uses **forward slashes** (`/`)
(e.g., `~/Downloads`). When you paste a Windows path into MobaXterm, you
**must change all backslashes to forward slashes**. Also, in MobaXterm,
your Windows `C:\` drive is typically accessible at `/drives/c/` or
`/mnt/c/` depending on your setup, so your Downloads folder would be
something like `/drives/c/Users/YourName/Downloads/` rather than
`~/Downloads/`.

------------------------------------------------------------------------

## 3. Navigating the File System

### Understanding File Paths

Every file and folder on your computer has an **address**, called a
**path**. Think of it like a postal address — it tells the computer
exactly where to find something. There are two types of paths:

**Full (Absolute) Path:** This gives the complete address starting from
the very top of the file system, called the root. On Mac and Linux, the
root is `/`. A full path always starts with `/` (or `~` which is a
shortcut for your home directory).

``` bash
/Users/yourname/Downloads/Genomic_files/example_file_01.fasta    # Mac absolute path
~/Downloads/Genomic_files/example_file_01.fasta                  # Using ~ shortcut
```

**Relative Path:** This gives the address *relative to where you
currently are* in the file system. It does not start with `/` or `~`.

``` bash
Genomic_files/example_file_01.fasta      # if you are already in ~/Downloads
example_file_01.fasta                    # if you are already in ~/Downloads/Genomic_files
```

### When to Use Which?

Use a **full path** when you want a command to work regardless of where
you currently are, when writing scripts that will be run from different
locations, or when you are unsure of your current location. Use a
**relative path** when you are already working inside the relevant
directory and the path is shorter and easier to type. In bioinformatics
scripts, full paths are generally safer and more reproducible.

### The Home Directory (`~`)

The tilde symbol `~` is a special shortcut that always refers to your
home directory (e.g., `/Users/yourname` on Mac or `/home/yourname` on
Linux). So `~/Downloads` always means the Downloads folder inside your
personal home folder, no matter where else you are in the file system.

------------------------------------------------------------------------

## 4. Essential Navigation Commands

### `pwd` — Print Working Directory

Before doing anything, it helps to know where you are. `pwd` prints the
full path of your current location.

``` bash
pwd
```

Example output:

    /Users/yourname

### `ls` — List Contents

`ls` lists the files and folders in your current directory.

``` bash
ls
```

You can also list the contents of a specific folder without navigating
to it:

``` bash
ls ~/Downloads/Genomic_files
```

Useful flags to add:

``` bash
ls -l    # long format: shows permissions, size, date
ls -a    # show all files including hidden ones (those starting with .)
ls -lh   # long format with human-readable file sizes (e.g., KB, MB)
ls -F    # adds / after directory names, making them easy to spot
```

### `cd` — Change Directory

`cd` moves you from one location to another.

``` bash
cd ~/Downloads/Genomic_files    # navigate directly to the Genomic_files folder
cd ..                           # go up one level (to the parent directory)
cd                              # go back to your home directory
cd -                            # go back to the previous directory you were in
```

### Putting it Together: Exploring the Genomic Files Folder

``` bash
# Start from wherever you are and navigate to the files
cd ~/Downloads/Genomic_files

# Confirm where you are
pwd

# See what's inside
ls -lh
```

Expected output from `ls -lh`:

    -rw-r--r--  1 yourname  staff  3.2K  21 Feb  example_file_01.fasta
    -rw-r--r--  1 yourname  staff  5.8K  21 Feb  example_file_02.fastq

------------------------------------------------------------------------

## 5. Looking at File Contents

In bioinformatics you will often work with files that are far too large
to open in a text editor. Bash gives you several tools to peek inside
files without opening them fully.

### `cat` — Print Entire File

`cat` (concatenate) prints the entire contents of a file to the screen.
This is fine for small files but will flood your terminal for large
ones.

``` bash
cat ~/Downloads/Genomic_files/example_file_01.fasta
```

### `head` and `tail` — First or Last Lines

``` bash
head example_file_01.fasta          # prints the first 10 lines (default)
head -n 4 example_file_01.fasta     # prints the first 4 lines
tail example_file_02.fastq          # prints the last 10 lines
tail -n 8 example_file_02.fastq     # prints the last 8 lines
```

### `less` — Scrollable Preview

`less` lets you scroll through a file one screen at a time without
loading it all at once. This is the preferred tool for large genomic
files.

``` bash
less example_file_01.fasta
```

Inside `less`: use the arrow keys or `Space` to scroll down, `b` to
scroll back up, `/` followed by a search term to search, and `q` to
quit.

### `wc` — Word/Line Count

`wc` (word count) is useful for getting quick statistics about a file.

``` bash
wc -l example_file_01.fasta     # count the number of lines
wc -l example_file_02.fastq
```

For FASTA files, the number of sequences is half the number of lines
(each sequence has a header line and a sequence line). For FASTQ files,
each read takes up 4 lines, so the number of reads is the total lines
divided by 4.

------------------------------------------------------------------------

## 6. Understanding FASTA and FASTQ File Formats

Two of the most common file formats in genomics are FASTA and FASTQ.
Let’s explore both using our example files.

### FASTA Format

A FASTA file stores biological sequences (DNA, RNA, or protein). Each
entry has exactly two parts: a **header line** starting with `>` that
contains the sequence name and any description, followed by one or more
lines of sequence data.

Let’s look at the first four lines of our example:

``` bash
head -n 4 ~/Downloads/Genomic_files/example_file_01.fasta
```

Output:

    >gene01_highGC_long
    ATGCGTACGTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCGTACGTAGCTAGCTAGCTAGCGTACGTAGC...
    >gene02_with_Ns_long
    ATGCTAGCTAGCTAGCTAGCNNNNNNNNNNCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCN...

Notice a few things. The `>` symbol at the start of a line always marks
the beginning of a new sequence entry and its name. The sequence
`gene02_with_Ns_long` contains the letter `N`, which is a standard
notation meaning the base at that position is **unknown** — a common
occurrence in raw genome assemblies. `gene03` contains ambiguity codes
like `R`, `Y`, `S`, `W`, which represent positions where two possible
bases are equally likely (e.g., `R` means A or G, `Y` means C or T).

You can count how many sequences are in a FASTA file by counting the
number of lines that start with `>`:

``` bash
grep -c "^>" example_file_01.fasta
```

This uses `grep` (covered in the next section) and the `^` symbol, which
means “start of line.” This file has 20 gene sequences.

### FASTQ Format

A FASTQ file stores sequencing **reads** along with quality scores for
each base. It is the standard output format from next-generation
sequencing machines (like Illumina). Each read in a FASTQ file takes up
exactly **4 lines**:

1.  A header line starting with `@` containing the read name
2.  The DNA sequence
3.  A `+` separator line (sometimes repeated with the header)
4.  A quality score string — one character per base

``` bash
head -n 8 ~/Downloads/Genomic_files/example_file_02.fastq
```

Output:

    @read01_high_to_medium_random
    ATGCGTACGTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCGTACGTAGCTAGCTAGCTAGCGTACGTAGC...
    +
    IHIFHIGIHGIIHIIHFHGIHGGHIIHHGIIHGIHFHGFHGHIIHGGIHGIIHFHGIHGHIGHFHGIGIH...
    @read02_good_random
    ATGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGC...
    +
    HHFGHHEHFHGHGHHHGFHGHHGFHHGHHFHHGHHFGHGFHHHHGHHGHHGFHHHGHHGHHFGHHGHHHG...

**Quality scores** are encoded as ASCII characters. Each character maps
to a number representing the probability that the base call is wrong —
this is called the **Phred quality score**. A high Phred score (e.g.,
`I` = Phred 40) means the sequencer is very confident the base is
correct; a low score (e.g., `!` = Phred 0) means low confidence. Look at
`read05_medium_with_bad_tail` in the file:

``` bash
grep -A 3 "read05" ~/Downloads/Genomic_files/example_file_02.fastq
```

Output:

    @read05_medium_with_bad_tail
    ATATATATATATATATATATATATATATATATATATATATATATATATATATATATATATATATATATAT
    +
    EEEDEEFEDDDDEEDDEEDDEEDDDCEEDDEEDDEEDDEEDCCDDCCCCC:::::::9999888877776666555544////

You can see the quality scores drop towards the end of the read
(characters like `5`, `4`, `/` are low quality). This is typical of
Illumina sequencing, where quality tends to decrease towards the 3’ end
of a read. FASTQ files are the starting point for most bioinformatics
pipelines — quality control and trimming tools like FastQC and
Trimmomatic use this quality information.

### FASTA vs. FASTQ: A Summary

| Feature | FASTA | FASTQ |
|----|----|----|
| Lines per entry | 2 (header + sequence) | 4 (header, sequence, +, quality) |
| Header starts with | `>` | `@` |
| Contains quality scores | No | Yes |
| Typical use | Reference genomes, gene sequences | Raw sequencing reads from NGS |
| File extension | `.fasta`, `.fa`, `.fna` | `.fastq`, `.fq` |

------------------------------------------------------------------------

## 7. Working with Files and Directories

### `mkdir` — Make a New Directory

``` bash
mkdir my_analysis
mkdir -p results/trimmed/fastq     # -p creates parent directories if they don't exist
```

### `cp` — Copy Files

``` bash
cp example_file_01.fasta ~/Desktop/                         # copy to Desktop
cp example_file_01.fasta example_file_01_backup.fasta       # copy and rename
cp -r Genomic_files ~/Desktop/                              # -r copies a whole folder (recursive)
```

### `mv` — Move or Rename Files

``` bash
mv example_file_01.fasta ~/Desktop/                         # move to Desktop
mv old_name.fasta new_name.fasta                            # rename a file
```

> ‼️ Unlike copying, `mv` removes the file from the original location.
> There is **no undo** in Bash, so be careful.

### `rm` — Remove Files

``` bash
rm unwanted_file.txt              # delete a file — permanent, no Recycle Bin!
rm -r unwanted_folder/            # delete a folder and all its contents
```

> ‼️ **There is no Recycle Bin in Bash.** When you `rm` something, it is
> gone. Always double-check before pressing Enter.

------------------------------------------------------------------------

## 8. Searching with `grep`

`grep` (Global Regular Expression Print) searches for patterns inside
files and prints matching lines. It is one of the most useful tools in
bioinformatics.

``` bash
# Find all sequence headers in the FASTA file
grep ">" example_file_01.fasta

# Count the number of reads in the FASTQ file (lines starting with @)
grep -c "^@" example_file_02.fastq

# Search for sequences containing long runs of N (unknown bases)
grep "NNNNNNNNNN" example_file_01.fasta

# Show the header AND the sequence below it (-A 1 = show 1 line After match)
grep -A 1 "gene05" example_file_01.fasta

# Case-insensitive search
grep -i "atrich" example_file_01.fasta
```

------------------------------------------------------------------------

## 9. Redirecting Output and Pipes

By default, commands print their output to the screen. You can redirect
that output elsewhere using special symbols.

### `>` — Write Output to a File (Overwrites)

``` bash
grep ">" example_file_01.fasta > sequence_headers.txt
```

This saves all the FASTA headers into a new file called
`sequence_headers.txt`.

### `>>` — Append Output to a File

``` bash
grep ">" example_file_01.fasta >> all_headers.txt    # adds to the file rather than overwriting
```

### `|` — The Pipe

The pipe `|` takes the output of one command and sends it directly as
input to the next. This lets you chain commands together into powerful
one-liners.

``` bash
# Count how many sequences contain Ns in the FASTA file
grep "N" example_file_01.fasta | grep "^[^>]" | wc -l

# Look at just the first 5 read headers in the FASTQ file
grep "^@" example_file_02.fastq | head -n 5
```

------------------------------------------------------------------------

## 10. Wildcards

Wildcards let you refer to multiple files at once without typing each
name individually.

`*` matches any number of characters. `?` matches exactly one character.

``` bash
# List all FASTA files in the current directory
ls *.fasta

# List all files that start with "example"
ls example*

# Run head on all fastq files at once
head -n 4 *.fastq
```

------------------------------------------------------------------------

## 11. Getting Help

Every command has built-in documentation. There are two main ways to
access it:

``` bash
man ls         # opens the manual page for ls — press q to exit
ls --help      # prints a shorter help summary directly in the terminal
```

On MobaXterm, `man` pages may not always be available, so `--help` is a
reliable fallback.

------------------------------------------------------------------------

le, so `--help` is a reliable fallback.

------------------------------------------------------------------------

## 12. Quality control using FastQC

Here is a workflow on how to check the quality of the reads gotten from
a sequencing technology like Illumina or ONT. The `-o` flag tells fastqc
where to save the outputs

``` bash
# For me alone: Activate the bio_training environment
mamba activate bio_training 

# 1. Install fastqc using homebrew on a macOS - first you need homebrew (check the website online for more information)
brew install fastqc

# 2. Conda could be used to install across different platform
conda install -c bioconda fastqc
 
# 3. Make directory to hold the fastqc result
mkdir ~/Downloads/Genomic_files/fastqc_results
 
# 4. Run fastqc
fastqc -o ~/Downloads/Genomic_files/fastqc_results/ example_file_02.fastq

# 5. Unzip the zip files generated 
cd ~/Downloads/Genomic_files/fastqc_results     # Change directory to the results directory
ls -l                               # list to see the content of the directory
unzip example_file_02_fastqc.zip                # unzip the content of the directory

# 6. Use cat to view the content of the summary.txt
cat fastqc_results/example_file_02_fastqc/summary.txt
```

------------------------------------------------------------------------

## 13. A Practical Workflow: Exploring Your Genomic Files

Here is a complete example workflow putting everything together.
Starting fresh in your terminal:

``` bash
# 1. Go to the Genomic_files folder
cd ~/Downloads/Genomic_files

# 2. Confirm location and see what's there
pwd
ls -lh

# 3. How many sequences in the FASTA file?
grep -c "^>" example_file_01.fasta

# 4. How many reads in the FASTQ file?
grep -c "^@" example_file_02.fastq

# 5. How many reads contain Ns?
grep -c "^N\|N$\|NNN" example_file_02.fastq

# 6. Save all FASTA headers to a new file
grep "^>" example_file_01.fasta > fasta_headers.txt

# 7. Preview the quality of the first read
head -n 4 example_file_02.fastq

# 8. Scroll through the whole FASTQ interactively
less example_file_02.fastq
```

------------------------------------------------------------------------

## 14. Quick Reference Card

| Command               | What it does                                 |
|-----------------------|----------------------------------------------|
| `pwd`                 | Print current location                       |
| `ls`                  | List files in current or specified directory |
| `cd path`             | Move into a directory                        |
| `cd ..`               | Go up one directory                          |
| `cd ~`                | Go to home directory                         |
| `cat file`            | Print file contents                          |
| `head -n N file`      | Print first N lines                          |
| `tail -n N file`      | Print last N lines                           |
| `less file`           | Scroll through file (q to quit)              |
| `wc -l file`          | Count lines in a file                        |
| `grep "pattern" file` | Find lines matching a pattern                |
| `grep -c`             | Count matching lines                         |
| `grep -A N`           | Show N lines after each match                |
| `mkdir name`          | Create a new directory                       |
| `cp source dest`      | Copy a file                                  |
| `mv source dest`      | Move or rename a file                        |
| `rm file`             | Delete a file (permanent!)                   |
| `command > file`      | Save output to a file                        |
| `command >> file`     | Append output to a file                      |
| `cmd1 \| cmd2`        | Pipe output of cmd1 into cmd2                |
| `*`                   | Wildcard: match anything                     |
| `man command`         | Open manual page                             |
| `command --help`      | Short help text                              |

------------------------------------------------------------------------

*This guide is based on material from the [Data Carpentry Genomics
curriculum](https://datacarpentry.org/genomics-workshop/). Refer to the
official lessons for extended exercises and additional topics including
loops, shell scripts, and working on remote servers.*

*This material was written by me with formatting and structure
assistance from Claude.*
