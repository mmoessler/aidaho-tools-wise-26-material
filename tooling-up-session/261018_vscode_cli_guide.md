# VS Code and the Command Line: A Practical Guide

This guide introduces **Visual Studio Code (VS Code)** and the **command-line interface (CLI)** as general tools for AI and data-science work. The goal is not to turn you into a system administrator or VS Code expert. Instead, you should get in touch with a small, reliable workflow for working with project files, running commands, inspecting changes, and understanding instructions proposed by humans or AI systems.

> **Course perspective:** Jupyter/JupyterLab remains the primary beginner interface for Python, and RStudio remains the primary beginner interface for R. VS Code is an **optional, forward-looking universal interface** that connects source files, Markdown, terminals, and version-control workflows.

By the end of this guide, you should be able to:

- open a project folder in VS Code;
- recognize the editor, file explorer, and integrated terminal;
- navigate through files and directories from a terminal;
- create, copy, move, and inspect files using a few basic CLI commands;
- run a Python or R script from the command line;
- use `code .` to open a project and `code --diff` to compare two text files;
- explain why text-based workflows are useful for reproducibility and collaboration;
- inspect AI-generated commands before executing them.

---

## 0. Content

- [0. Content](#0-content)
- [1. VS Code: one interface for many tools](#1-vs-code-one-interface-for-many-tools)
  - [Installing VS Code](#installing-vs-code)
  - [Extensions](#extensions)
  - [Open a project folder](#open-a-project-folder)
- [2. Terminal, shell, and CLI](#2-terminal-shell-and-cli)
  - [Paths and directories](#paths-and-directories)
- [3. A small CLI toolkit](#3-a-small-cli-toolkit)
  - [A short navigation exercise](#a-short-navigation-exercise)
- [4. Running Python and R from the CLI](#4-running-python-and-r-from-the-cli)
  - [Why run scripts from the CLI?](#why-run-scripts-from-the-cli)
- [5. Markdown and text-based project documentation](#5-markdown-and-text-based-project-documentation)
- [6. Comparing files with code --diff](#6-comparing-files-with-code---diff)
- [7. CLI workflows with LLMs and AI agents](#7-cli-workflows-with-llms-and-ai-agents)
- [8. What to remember](#8-what-to-remember)

---

## 1. VS Code: one interface for many tools

[Visual Studio Code](https://code.visualstudio.com/) is a general-purpose source-code editor. Unlike Python, R, or a package manager, VS Code does **not itself provide the language interpreter** that executes your Python or R code. Instead, it provides an interface from which you can work with files and connect to interpreters and other tools installed on your computer.

A useful mental model is:

``` text
project files
    |
    +-- Python interpreter + packages
    +-- R interpreter + packages
    +-- other tools
    |
    v
VS Code
    |
    +-- editor
    +-- file explorer
    +-- integrated terminal
    +-- extensions
    +-- source-control / diff tools
```

This distinction matters when troubleshooting. If a Python program fails because a package is missing, reinstalling VS Code is usually not the solution: the relevant question is **which Python interpreter/environment is running the program and which packages are installed there**.

---

### Installing VS Code

Download VS Code from its official website and follow the installer for your operating system:

- Windows: use the standard Windows installer;
- macOS: install the application and move it to the Applications folder if requested;
- Linux: use the package provided for your distribution or follow the official installation instructions.

After installation, open VS Code once and familiarize yourself with three areas:

1. **Explorer**: Displays the files and directories in the opened project.
2. **Editor**: Displays and edits the selected text or source-code file.
3. **Terminal**: Provides a command-line interface inside VS Code.

The exact appearance can vary with the operating system and VS Code version.

---

### Extensions

VS Code can be extended to support different languages and file types. Useful extensions for this course include support for:

- Python;
- Jupyter notebooks;
- Markdown
- R;
- Quarto.

Do not install large collections of extensions simply because they are available. Start with what you actually need. An extension is another software component and can therefore also affect your workflow or configuration.

> **Important:** An extension for a language normally does not replace the language itself. For example, a Python extension for VS Code does not remove the need for an accessible Python installation/interpreter.

---

### Open a project folder

A good habit is to work with a **project directory** rather than opening unrelated files individually. For example:

``` text
tooling-up/
├── README.md
├── analysis.py
├── analysis.R
├── data/
└── results/
```

In VS Code, use **File → Open Folder...** and select the project directory.

If the VS Code command-line launcher is available, the same action can be expressed explicitly from a terminal:

``` bash
cd path/to/tooling-up
code .
```

Here, `.` means **the current directory**.

If `code` is not recognized as a command, VS Code is still usable through its graphical interface. The command-line launcher may need to be added to your system path; consult the current VS Code documentation for your operating system.

---

## 2. Terminal, shell, and CLI

A **terminal** is an application or interface in which you type textual commands. A **shell** is the program that interprets those commands. The **command-line interface (CLI)** is the text-based way in which you interact with a program or operating system.

For our purposes, you do not need to master the differences between shells. You only need to recognize that command syntax can differ slightly between operating systems.

Common examples include:

- **Windows:** PowerShell or Command Prompt;
- **macOS:** Terminal, commonly using `zsh`;
- **Linux:** a terminal commonly using `bash` or another shell;
- **Git Bash on Windows:** an optional Unix-like shell installed with Git for Windows.

> **Windows:** We recommend installing Git for Windows and using Git Bash, since most CLI commands are typically provided in Unix-like shells.

VS Code contains an **integrated terminal**. Open it using **Terminal → New Terminal**. It normally starts in the directory of your current project, making it convenient to connect the files you see in the editor with commands that operate on those files.

---

### Paths and directories

A **path** describes the location of a file or directory.

Examples:

``` text
data/survey.csv
results/figure.png
scripts/analysis.py
```

A **relative path** is interpreted relative to the current directory. An **absolute path** describes a location starting from the root of the file system.

Paths may look different across operating systems. Windows commonly uses forms such as:

``` text
C:\Users\Student\project
```

macOS and Linux commonly use forms such as:

``` text
/Users/student/project
/home/student/project
```

For reproducible projects, relative paths such as `data/survey.csv` are often preferable to hard-coded paths that only exist on one person's computer.

---

## 3. A small CLI toolkit

The following commands are enough for many basic course workflows. Some file-management commands differ between Windows shells and macOS/Linux shells.

| Task | macOS/Linux/Git Bash | Windows Command Prompt | Typical PowerShell |
|---|---|---|---|
| show current directory | `pwd` | `cd` | `Get-Location` or `pwd` |
| list files | `ls` | `dir` | `Get-ChildItem` or `ls` |
| change directory | `cd folder` | `cd folder` | `cd folder` |
| go up one directory | `cd ..` | `cd ..` | `cd ..` |
| create directory | `mkdir results` | `mkdir results` | `mkdir results` |
| copy a file | `cp a.txt b.txt` | `copy a.txt b.txt` | `Copy-Item a.txt b.txt` |
| move/rename a file | `mv a.txt b.txt` | `move a.txt b.txt` | `Move-Item a.txt b.txt` |
| delete a file | `rm a.txt` | `del a.txt` | `Remove-Item a.txt` |

You do **not** need to memorize all variants. The important ideas are that commands have a name, may receive arguments, and operate relative to a current directory.

---

### A short navigation exercise

Create or choose a practice directory and try:

``` bash
pwd
ls
mkdir tooling-practice
cd tooling-practice
pwd
```

On Windows Command Prompt, use `cd` to display the current directory and `dir` to list files if `pwd` and `ls` are unavailable.

Now create a file called `README.md` in VS Code and save it in `tooling-practice`. Return to the terminal and list the directory contents. You should see the file.

This small exercise connects three concepts:

``` text
file system  <-->  VS Code Explorer/editor  <-->  terminal/CLI
```

They are different interfaces to the same project files.

---

## 4. Running Python and R from the CLI

A source-code file is text. To execute it, you invoke the relevant interpreter.

For Python:

``` bash
python analysis.py
```

Depending on your operating system and installation, the command may instead be `python3`. If you use Conda, the Python interpreter also depends on the currently active Conda environment.

A useful diagnostic command is:

``` bash
python --version
```

For R, a script can be executed non-interactively with:

``` bash
Rscript analysis.R
```

You can inspect the R version with:

``` bash
R --version
```

If `python`, `R`, or `Rscript` is reported as an unknown command, this does not necessarily mean the program is absent. It can also mean that the shell cannot find the executable through its configured search path.

> **Anaconda:** Installing Python using Anaconda adds another layer called **Conda environments**. Conda environments allow you to manage and use different Python versions and different versions of installed Python packages. We do not have to deal with multiple environments in this introductory course. Therefore, you can simply use the **Conda `base` environment**, which is the default environment that comes with Anaconda.
>
> Depending on your installation configuration, the `base` environment is either activated automatically—indicated by `(base)` at the beginning of the command-line prompt—or needs to be activated manually using:
>
> `conda activate base`

---

### Why run scripts from the CLI?

The command

``` bash
python analysis.py
```

is a compact, explicit description of an action. Compared with a sequence such as "open this menu, click that button, then select this file," it is easy to:

- copy;
- document;
- repeat;
- automate;
- share with another person;
- propose or inspect in an LLM-assisted workflow.

This does **not** mean that a CLI is always better than a graphical interface. It means that textual commands are particularly useful when an action should be explicit and reproducible.

---

## 5. Markdown and text-based project documentation

Markdown files such as `README.md` are plain-text files with lightweight formatting. For example:

``` markdown
# My data project

## Goal

Investigate ...

## Files

- `analysis.py`
- `data/survey.csv`
- `results/`
```

The same basic Markdown ideas recur in Jupyter notebook Markdown cells, Quarto documents, README files, and many web-based development platforms.

Plain text has several useful properties: it is portable, searchable, easy to edit, easy to generate, and---most importantly for collaborative workflows---easy to **compare**.

In VS Code, open `README.md`, change one sentence, and save the file. You have now created two conceptual states: the version before the edit and the version after it. A **diff** is a representation of the differences between such states.

---

## 6. Comparing files with code --diff

Suppose you have two files:

``` text
documentation-v1.md
documentation-v2.md
```

VS Code can open a difference editor from the command line:

``` bash
code --diff documentation-v1.md documentation-v2.md
```

The editor highlights which lines differ. This gives a concrete answer to the question:

> **What changed?**

Try the following exercise:

1. Create `documentation-v1.md`.
2. Copy it to `documentation-v2.md`.
3. Modify two or three lines in the second file.
4. Run `code --diff documentation-v1.md documentation-v2.md`.
5. Identify additions, removals, and modifications.

A diff is useful even without using a version control software like Git. It introduces the central idea behind version control: changes to text files can be represented explicitly and reviewed.

The conceptual progression is:

``` text
plain-text files
      |
      v
changes are explicit
      |
      v
diff: what changed?
      |
      v
history: when and why?
      |
      v
restore: can we recover an earlier state?
      |
      v
version control / Git
```

Git is the standard tool for systematically tracking and managing these changes. Topics such as branches, merging, remotes, and collaborative GitHub/GitLab workflows are part of the standard workflow when working on and collaborating on ML or DS projects.

---

## 7. CLI workflows with LLMs and AI agents

LLMs and AI agents can often generate shell commands, edit source files, or propose changes to a project. Text-based tools make these actions comparatively easy to inspect.

For example, an AI system might propose:

``` bash
python analysis.py
```

or:

``` bash
quarto render report.qmd
```

These commands communicate a concrete intended action. However, **a command should not be executed merely because an AI system generated it**.

Before running an unfamiliar command, ask:

1. **What program will this command run?**
2. **Which files or directories will it read?**
3. **Will it create, overwrite, move, or delete anything?**
4. **Does it install software or packages?**
5. **Does it access the network or send data elsewhere?**
6. **Do I understand the important options and paths?**

Be particularly careful with commands that delete files, recursively modify directories, change permissions, install software, or run downloaded code.

> **AI-literacy principle:** LLMs and AI agents can propose commands, but the human user should understand and inspect a command before executing it.

The same principle applies when an AI system edits project files. A diff lets you inspect **what changed**; version control provides history and recovery Together, these tools support a workflow in which AI can assist while the human retains oversight.

---

## 8. What to remember

You do not need to become a VS Code or shell expert. The key ideas are:

- **VS Code is an interface**, not the Python or R interpreter itself.
- **The terminal connects textual commands to your project files and tools.**
- **The current directory matters** because relative paths and commands are interpreted from it.
- **CLI commands make actions explicit and reproducible.**
- **Markdown and source code are plain text**, which makes changes easy to inspect.
- **A diff answers "what changed?"**
- **Git extends this idea with systematic history and recovery.**
- **AI-generated commands and edits should be inspected rather than trusted blindly.**

The desired outcome is a workflow that is not only functional, but also **explicit, inspectable, reproducible, and transferable between humans and AI systems**.
