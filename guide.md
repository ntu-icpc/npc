---
title: Guide
layout: page
nav_order: 100002
---

# Guide

## Visual Studio Code

Visual Studio Code (VS Code) is a lightweight but powerful source code editor that runs on your desktop. It comes with built-in support for JavaScript, TypeScript, and Node.js and has a rich ecosystem of extensions for other languages and tools.

### Installation

- Visit [https://code.visualstudio.com/](https://code.visualstudio.com/)
- Download the appropriate version for your operating system
- Follow the installation instructions

### Opening the Terminal in VS Code

VS Code has an integrated terminal that allows you to execute command-line operations without leaving the editor.

1. **Ways to open the terminal**:
   - Use the keyboard shortcut: 
     - Windows/Linux: `Ctrl + ` (backtick)
     - macOS: `Cmd + ` (backtick)
   - From the menu: View > Terminal
   - Use the command palette (`Ctrl+Shift+P` or `Cmd+Shift+P`), then type "Terminal: Create New Terminal"

2. **Terminal features**:
   - Multiple terminals: Click the + icon to create additional terminals
   - Split terminals: Use the split icon to divide the terminal panel
   - Terminal selection: Use the dropdown to switch between terminals
   - Terminal customization: Configure in Settings (`Ctrl+,` or `Cmd+,`)

### Compiling and Running Code from the Terminal

#### C

1. **Compile a C program**:
   ```bash
   gcc -o program_name source_file.c -Wall -g
   ```
   - `-Wall`: Enable all warnings
   - `-g`: Include debugging information
   - `-o program_name`: Specify the output file name
   - More details can be found in [GCC Options Summary](https://gcc.gnu.org/onlinedocs/gcc/Option-Summary.html).

2. **Run the compiled program**:
   ```bash
   ./program_name
   ```

3. **Compile with additional flags**:
   ```bash
   gcc -Wall -g -o program_name source_file.c
   ```
   - `-Wall`: Enable all warnings
   - `-g`: Include debugging information

#### C++

1. **Install a C++ compiler**:
   - Windows: Install MinGW or MSVC
   - macOS: Install Xcode Command Line Tools
   - Linux: Install G++ using your package manager (e.g., `sudo apt install g++`)

2. **Compile a C++ program**:
   ```bash
   g++ -o program_name source_file.cpp
   ```

3. **Run the compiled program**:
   - Windows: `program_name.exe`
   - macOS/Linux: `./program_name`

4. **Compile with C++ standard specification**:
   ```bash
   g++ -std=c++17 -o program_name source_file.cpp
   ```

#### Java

1. **Install JDK (Java Development Kit)**:
   - Download and install from [Oracle](https://www.oracle.com/java/technologies/javase-downloads.html) or use OpenJDK

2. **Compile a Java program**:
   ```bash
   javac FileName.java
   ```
   This creates a `FileName.class` file

3. **Run a Java program**:
   ```bash
   java FileName
   ```
   Note: Do not include the `.class` extension when running

4. **Compile with classpath**:
   ```bash
   javac -cp path/to/libraries FileName.java
   ```

#### Python

1. **Install Python**:
   - Download from [python.org](https://www.python.org/downloads/)
   - Ensure it's added to your PATH during installation

2. **Run a Python script**:
   ```bash
   python script.py
   ```
   or
   ```bash
   python3 script.py
   ```
   (depending on your installation)

3. **Python doesn't require compilation** in the traditional sense, but you can:
   - Check syntax without running:
     ```bash
     python -m py_compile script.py
     ```
   - Create a bytecode file:
     ```bash
     python -c "import py_compile; py_compile.compile('script.py')"
     ```

#### Go

1. **Install Go**:
   - Download from [golang.org](https://golang.org/dl/)
   - Follow the installation instructions for your OS

2. **Compile and run in one step**:
   ```bash
   go run main.go
   ```

3. **Compile a Go program**:
   ```bash
   go build main.go
   ```
   This creates an executable file

4. **Run the compiled program**:
   - Windows: `main.exe`
   - macOS/Linux: `./main`

### VS Code Extensions for Better Development

For each language, consider installing these helpful extensions:

- **C/C++**: Microsoft C/C++ extension
- **Java**: Extension Pack for Java
- **Python**: Python extension by Microsoft
- **Go**: Go extension by Go Team at Google

### Debugging in VS Code

1. Set breakpoints by clicking in the gutter next to line numbers
2. Press F5 to start debugging (configure launch.json if prompted)
3. Use the debug toolbar to step through code, inspect variables, etc.

### Useful VS Code Shortcuts

- `Ctrl+S` / `Cmd+S`: Save file
- `Ctrl+X` / `Cmd+X`: Cut line
- `Ctrl+C` / `Cmd+C`: Copy line
- `Ctrl+V` / `Cmd+V`: Paste
- `Ctrl+Z` / `Cmd+Z`: Undo
- `Ctrl+Shift+Z` / `Cmd+Shift+Z`: Redo
- `Ctrl+F` / `Cmd+F`: Find
- `Ctrl+H` / `Cmd+H`: Replace
- `F5`: Start debugging
- `Shift+F5`: Stop debugging
- `F11`: Step into
- `F10`: Step over
