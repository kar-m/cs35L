# Linux and System Programming – CS 35L Notes

## 1. Overview of Linux System Architecture

### 1.1. Hardware and Kernel
- The **Linux operating system** runs on **x86-64** hardware.
- The **Linux kernel** is responsible for managing system resources and handling requests from applications.
- The kernel provides:
  - **Process management** (scheduling, execution, termination).
  - **Memory management** (allocating, paging, swapping).
  - **File system management** (reading, writing, permissions).
  - **Hardware communication** (drivers for devices like keyboards, GPUs, network cards).
  - **System calls** for user programs to interact with the kernel.

### 1.2. User-Space vs. Kernel-Space
- **User-space**:
  - Where applications run (e.g., browsers, editors).
  - Restricted from accessing hardware directly.
- **Kernel-space**:
  - Executes system processes, drivers, and low-level system functions.
  - Can perform **privileged operations** like interacting with hardware (halt).

### 1.3. System Calls
- Programs interact with the **kernel** via **system calls**.
- Examples of system calls:
  - **File operations**: `open()`, `read()`, `write()`, `close()`
  - **Process control**: `fork()`, `exec()`, `exit()`, `kill()`
  - **Memory management**: `mmap()`, `brk()`
  - **Networking**: `socket()`, `bind()`, `connect()`
- **Command to trace system calls of a process:**
  ```sh
  strace ls

# 2. Memory and Libraries
## 2.1. Memory Operations
- **Memory operations in C**:
  - memcpy(dest, src, nbytes): Copies nbytes from src to dest efficiently.
- **Memory allocation functions**:
  - malloc(size): Allocates size bytes.
  - free(ptr): Releases memory.
  - calloc(n, size): Allocates and initializes memory.
  - realloc(ptr, new_size): Resizes allocated memory.
## 2.2. Shared Libraries
- Shared libraries reduce redundancy and allow multiple programs to use the same functions.
- **Finding shared libraries used by a program:**
```sh
ldd /usr/bin/bash
```
- Key shared libraries:
  - glibc → Standard C library.
  - libm → Math library.
  - libpthread → Threading library.
- **Dynamic linking**: Uses shared libraries at runtime.
- **Static linking**: Copies library functions directly into the executable.
# 3. Linux File System
## 3.1. Filesystem Structure
- **Linux follows a hierarchical file structure.**
- **Key directories:**
  - / → Root directory.
  - /home/ → Contains user directories (/home/username).
  - /bin/ → Essential command binaries (ls, cat, grep).
  - /usr/bin/ → Non-essential command binaries.
  - /etc/ → System-wide configuration files.
  - /tmp/ → Temporary files (deleted on reboot).
  - /var/ → Variable data (logs, databases).
  - /proc/ → Virtual filesystem for process and system information.
  - /dev/ → Device files (e.g., sda1 for disk partitions).
  - /mnt/ & /media/ → Mount points for external drives.
## 3.2. File Names and Constraints
- **File Naming Rules:**
  - A file name is a sequence of non-empty bytes.
  - Cannot contain / or \0 (null byte).
  - File name length limit: Typically 255 bytes.
  - Some special cases:
    - "." refers to the current directory.
    - ".." refers to the parent directory.
## 3.3. File Permissions
- **Files and directories have permissions assigned for:**
  - Owner (user who created the file).
  - Group (users within the same group).
  - Others (everyone else).
- **Permission format:**
```sh
-rwxrw-r--
```
  - - → Regular file (d for directory, l for symbolic link).
  - rwx → Owner permissions (read, write, execute).
  - rw- → Group permissions.
  - r-- → Other users' permissions.
- **Changing permissions** (chmod):
```sh
chmod 755 filename   # Owner: read/write/execute, others: read/execute
chmod g+w file       # Add write permissions to the group
```
## 3.4. Hard Links vs. Symbolic Links
**Hard Links**
- A hard link is another name for the same file.
- Both names share the same inode number.
- **Commands:**
```sh
ln file1 file2    # Create a hard link
ls -li file1 file2  # Check inode numbers
```
**Symbolic Links**
- A symbolic link (symlink) is a pointer to another file.
- Unlike hard links, symlinks can point to directories and external filesystems.
- **Commands**:
```sh
ln -s target linkname  # Create a symbolic link
ls -l linkname         # Shows arrow pointing to target
```
# Symbolic Links - Key Concepts & Notes

## Definition & Basics
- Symbolic links (symlinks) are special file types that **point to another file or directory**.
- Unlike hard links, symlinks can point to files that **do not exist yet**.
- The contents of a symlink are a **byte string** representing a file path that gets interpreted when accessed.

## Differences Between Symbolic and Hard Links
| Feature            | Symbolic Links (Symlinks) | Hard Links |
|--------------------|-------------------------|------------|
| Can cross file system boundaries? | ✅ Yes | ❌ No |
| Can link to directories? | ✅ Yes | ❌ No |
| Can reference non-existent files? | ✅ Yes | ❌ No |
| Stores actual data? | ❌ No (only path info) | ✅ Yes (same inode as original) |
| Affected if the target is deleted? | ✅ Yes (becomes broken) | ❌ No (file remains accessible) |

## Behavior & Commands
### Creating and Listing Symbolic Links
```sh
ln -s <target> <link_name>   # Creates a symbolic link
ln <existing_file> <new_link> # Creates a hard link
ls -l <symlink>              # Shows where a symlink points
ls -ld /bin                  # Shows details about /bin (often a symlink)
df                           # Lists all file systems (each a separate tree)
```

### Example: Resolving Symlinks
```sh
ls -l /usr/bin/sh
```
This may resolve to:
```
/usr/bin/sh -> /usr/bin/bash
```
And then:
```
/usr/bin/bash -> /bin/bash
```
Until a real file (`/bin/bash`) is found.

## How Symlinks Work When Accessed
- Accessing a symlink causes the system to replace it with its stored path.
- If the symlink contains an **absolute path**, the system starts at the root (`/`) and follows the path as if it were typed directly.
- If the symlink contains a **relative path**, the system interprets it relative to the directory containing the symlink.
- If a symbolic link points to another symbolic link, this resolution process continues until a real file or directory is found (or until the system detects excessive recursion).

## Relative vs. Absolute Symlinks
- **Absolute Symlinks:** Start from the root (`/`), e.g., `/usr/bin/bash`.
- **Relative Symlinks:** Start from the symlink's location, e.g., `bin/sh` (relative to where the symlink is).

## Edge Cases & Errors
- A symlink pointing to itself (`ln -s loop loop`) creates an infinite loop, but Linux limits recursion.
- Accessing a symlink to a non-existent file (`ls -l nowhere`) results in a **"No such file or directory"** error.
- Empty symlinks (`ln -s '' empty`) may behave unexpectedly.

## Practical Example
```sh
ln -s usr/bin bin
ls -l bin
```
- Creates a symbolic link named `bin` pointing to `usr/bin`.
- `ls -l bin` shows where it points.

## Summary
- Symbolic links are flexible and commonly used for organizing files and managing file system structures.
- They enable portability and maintainability but should be used carefully to avoid broken links or infinite loops.

## 3.5. Finding File Metadata
**View file permissions and metadata:**
```sh
ls -l filename
```
**Find inode number:**
```sh
ls -i filename
```
**Find disk usage:**
```sh
df -h .
```
# 4. Processes and System Calls
## 4.1. Process Management
**Listing running processes:**
```sh
ps -ef  # List all processes
```
**Killing a process:**
```sh
kill PID  # Kill process by ID
```
Find process ID (PID):
```sh
ps -ef | grep process_name
```
## 4.2. /proc Filesystem
- Special directory for accessing system and process information.
- Useful commands:
```sh
ls -l /proc/PID/  # View process details
cat /proc/cpuinfo  # View CPU information
```
# 5. Shell and Command Execution
## 5.1. Navigation
- Print working directory:
```sh
pwd
```
- Change directory:
```sh
cd /path
```
- List files:
```sh
ls -l
```
## 5.2. File Manipulation
- Create an empty file:
```sh
touch filename
```
- Remove a file:
```sh
rm filename
```
- Remove a directory:
```sh
rm -r directory
```
## 5.3. Input/Output Redirection
- Redirect output to a file:
```sh
command > output.txt
```
- Redirect input from a file:
```sh
command < input.txt
```
- Pipe output to another command:
```sh
command1 | command2
```
# 2. Emacs

## Introduction to Emacs
Emacs is a powerful and extensible text editor that allows users to perform a variety of tasks beyond simple text editing. It includes support for windowing systems, scripting, and running subshells.

### Exiting Emacs
- To exit Emacs, type `C-x C-c` (Control + x followed by Control + c).

### Starting Emacs in Non-Graphical Mode
- Use `emacs -nw` to start Emacs in a non-graphical user interface (default in some systems like SEASnet).
- This is useful when setting up servers remotely, as graphical interfaces can increase latency.

## Emacs Keyboard Shortcuts and Commands
### Basic Navigation
- `C-x d` - Opens a directory editor, allowing navigation and file editing.
- `C-v` - Scrolls down a screenful.
- `C-p` - Moves up a line.
- `C-f` - Moves forward (right).
- `C-e` - Moves to the end of a line.
- `C-a` - Moves to the start of a line.
- `C-y` - Yanks (pastes) from the kill buffer.
- `C-d` - End-of-file (EOF) in shell mode.
- `C-x C-f` - Find and open a file.
- `C-x C-s` - Save the current buffer.

### Working with Buffers and Windows
- `C-x o` - Switches to another buffer.
- `C-x 2` - Splits the window horizontally.
- `C-x 3` - Splits the window vertically.
- `C-x 1` - Focuses on the current buffer, closing others.
- `C-x 0` - Deletes the current window.
- `C-x C-b` - Lists buffers.
- `C-x s` - Saves all open buffers interactively.
- `C-x C-c` - Exits Emacs.

### Searching and Editing
- `C-s` - Incremental search (like Ctrl+F in VS Code).
- `C-x C-h` - Marks the whole buffer.
- `C-k` - Kills (cuts) everything from the cursor to the end of the line.
- `M-w` - Copies from the marked region.
- `C-@` - Places a mark.

## Running a Shell in Emacs
- `M-x shell RET` - Starts a subshell running inside Emacs.
- `pwd` - Shows the current directory inside the Emacs shell.

## File System and Directory Editing
- `/` - Represents the root directory of the file system.
- In Emacs, you can provide a textual description of directories.
- File name components are a finite byte sequence with some restrictions:
  - Cannot contain `/` or `NULL` byte (`\0`).
  - Cannot be empty.
  - Some file systems limit the name length to 255 bytes.

### File Permissions and Symbolic Links
- `ls -l` - Lists files and their metadata.
- `chmod g+w ~/.` - Grants write permissions to a group.
- `M-x man` - Opens the manual pages.
- `chmod 755 ~/.` - Adjusts directory permissions.
- `ln -s` - Creates a symbolic link.
- `C-x d` - Opens a directory editor in Emacs.

## Advanced Features
### Emacs Lisp (Elisp)
- Emacs contains an extension language called Emacs Lisp (Elisp), which allows users to modify and extend functionality.
- Some commands for evaluating Elisp:
  - `M-:` - Evaluate an expression interactively.
  - `(setq variable value)` - Assigns a value to a variable.
  - `(defun function-name (args) body)` - Defines a function.

### Viewing Key Bindings
- `C-h k` - Displays the function associated with a keystroke.
- `C-h t` - Opens the tutorial.
- `C-h info` - Opens the Info documentation system.

### Miscellaneous
- `C-x C-c` - Exit Emacs.
- `C-x 5` - Creates a new frame.
- `M-x view-lossage` - Shows recent key commands.
- `C-h k` - Shows information about a specific keystroke.
- `C-x d` - Edit a directory.
- `M-x mark-whole-buffer` - Marks the entire buffer.

### Enabling Shell Features in Emacs
- Some keyboard shortcuts that work in the shell can also be enabled in Emacs.
- If using `iterm`, you may need to configure it to interpret `Option` as `Alt` to enable certain commands.

---
# Shell Scripting Notes

## Introduction to Shell Scripting
Shell scripting allows users to automate commands in a UNIX-based operating system. It is a powerful tool for managing files, processes, and system tasks.

## Basic Shell Commands
### Running a Shell in Emacs
- **M-x shell RET**: Starts a subshell inside Emacs.
- **pwd**: Shows the current directory.

### File System Navigation
- **cd [directory]**: Changes the current directory.
- **cd** (without arguments): Moves to the home directory.
- **ls -l [directory]**: Lists directory contents with details.
- **ls -a**: Shows all files, including hidden files.

### File Permissions and Ownership
- **ls -l**: Displays file metadata, including permissions.
- **chmod [permissions] [file]**: Changes file permissions.
  - `chmod 755 ~/.` → Grants read, write, and execute permissions.
  - `chmod ug+x file` → Grants execute permission to the user and group.
- **id**: Displays user ID and group ID.

### File Manipulation
- **touch [filename]**: Creates an empty file.
- **ln [source] [destination]**: Creates a hard link.
- **ln -s [source] [destination]**: Creates a symbolic (soft) link.
- **rm [filename]**: Deletes a file.

### Input and Output Redirection
- **command > file**: Redirects standard output to a file.
- **command < file**: Redirects input from a file.
- **command 2> error.log**: Redirects standard error to a file.
- **command > output 2>&1**: Merges standard output and standard error.

### Pipes and Process Management
- **command1 | command2**: Pipes output of command1 as input to command2.
- **grep 'pattern' file**: Searches for a pattern in a file.
- **ps -ef**: Lists all running processes.
- **kill [PID]**: Terminates a process.

## Shell Scripting Basics
A shell script is a series of commands written in a file that can be executed like a program.

### Creating a Shell Script
1. Create a script file:
   ```sh
   touch script.sh
   ```
2. Make it executable:
   ```sh
   chmod +x script.sh
   ```
3. Add a shebang to specify the shell interpreter:
   ```sh
   #!/bin/sh
   ```

### Variables and Arguments
- **Variable assignment**:
  ```sh
  name="John"
  echo $name
  ```
- **Special variables**:
  - `$?` → Exit status of the last command.
  - `$0` → Name of the script.
  - `$@` → All arguments passed to the script.
  - `$#` → Number of arguments.

### Conditionals
```sh
if [ "$1" = "hello" ]; then
    echo "Hello, World!"
else
    echo "Goodbye!"
fi
```

### Loops
#### For Loop:
```sh
for i in {1..5}; do
    echo "Iteration $i"
done
```
#### While Loop:
```sh
count=0
while [ $count -lt 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

### Functions
```sh
my_function() {
    echo "This is a function"
}
my_function
```

## Advanced Shell Topics
### Globbing (Filename Expansion)
- `*` → Matches any number of characters.
- `?` → Matches a single character.
- `[abc]` → Matches one character from the set.

### Regular Expressions in Grep
- `grep -E 'pattern' file` → Uses extended regular expressions.
- `grep '^start' file` → Matches lines starting with "start".
- `grep 'end$' file` → Matches lines ending with "end".

### Process Substitution
```sh
diff <(ls dir1) <(ls dir2)
```

### Exit Codes and Error Handling
- `exit 0` → Indicates success.
- `exit 1` → Indicates an error.
- **Error handling example**:
  ```sh
  if ! command; then
      echo "Error occurred"
      exit 1
  fi
  ```

### Background Processes
- **Run a command in the background**:
  ```sh
  long_running_command &
  ```
- **View background jobs**:
  ```sh
  jobs
  ```
- **Bring a background job to the foreground**:
  ```sh
  fg %1
  ```

## Shell Scripting in Practice
Shell scripts are used for:
- Automating system administration tasks.
- Managing files and directories.
- Processing text and logs.
- Scheduling tasks using cron jobs.

### Example: Backup Script
```sh
#!/bin/sh
tar -czf backup.tar.gz /home/user/documents
echo "Backup completed!"
```

## Distributed Systems & Version Control

### Distributed Computing Principles
- Distributed systems involve multiple computers (nodes) working together to achieve a common goal.
- The **Client-Server Model** is the most common type of distributed computing.
- **Peer-to-Peer Systems (P2P)** allow nodes to communicate directly without a central server.
- **Primary-Secondary Architecture**: The primary node controls the workload, while secondary nodes assist in computation.

### Version Control Principles
- **Version Control Systems (VCS)** track changes in software code over time.
- Git is a distributed VCS that enables multiple contributors to work on the same project while maintaining revision history.
- **Key Version Control Features**:
  - Branching and Merging
  - Commit history tracking
  - Remote repositories for collaboration

### Performance Tradeoffs
- **Throughput**: Measures how much useful work a system does per second.
- **Latency**: The delay between a request and response.
- **Tradeoffs**: Increasing throughput often increases latency, and vice versa.

### Caching Issues & Data Consistency
- **Caching** improves performance by storing frequently accessed data.
- Issues arise when caches are **outdated** (stale data) or **inconsistent** with the source.
- **Cache validation** techniques ensure caches are up-to-date.

---

## Networking and Internet Protocols

### Packet Switching vs. Circuit Switching
- **Circuit Switching** (used in early telephone networks) reserves a dedicated path for communication.
- **Packet Switching** (used in the Internet) breaks data into packets that travel independently.

### Internet Protocol (IP, IPv4, IPv6)
- **IPv4** uses 32-bit addresses, written as `192.168.1.1`.
- **IPv6** uses 128-bit addresses, allowing more unique addresses.

### Transport Layer Protocols
- **TCP (Transmission Control Protocol)** ensures reliable, ordered, and error-checked communication.
- **UDP (User Datagram Protocol)** is a lightweight alternative that does not guarantee delivery.
- **QUIC** (developed by Google) reduces latency and improves performance over TCP.

### Routing & Latency Concerns
- **Routers** direct packets through the internet based on optimal paths.
- **Latency** can be affected by network congestion, physical distance, and processing time at routers.

### Jitter & Real-time Communication (RTP)
- **Jitter** refers to variation in packet arrival times, causing delays in real-time applications like video calls.
- **RTP (Real-time Transport Protocol)** runs over UDP to minimize jitter.

### Client-Server Communication
- Servers process requests from multiple clients.
- Load balancing techniques improve efficiency.

### DNS Round-Robin Load Balancing
- Distributes network traffic by rotating between multiple server addresses.

### Common Networking Issues
- **Packet loss**: Data is dropped due to congestion or errors.
- **Out-of-order packets**: Packets arrive in a different order than sent.

---

## Web Development & Technologies

### HTTP Versions
- **HTTP/1.1** (1997): Supports persistent connections.
- **HTTP/2** (2015): Introduced multiplexing and header compression.
- **HTTP/3** (2022): Uses QUIC over UDP to reduce latency.

### Browser Rendering Pipeline
- Browsers incrementally render webpages while loading content.
- **Optimizations**:
  - Prioritizing visible elements.
  - Guessing page layout before full load.

### HTML Structure & Syntax
- **HTML (HyperText Markup Language)** structures web pages using tags.
- **XML vs. JSON**:
  - **XML**: Extensible markup, complex and used for documents.
  - **JSON**: Lightweight, human-readable, used for data exchange.

### Document Object Model (DOM)
- A structured representation of HTML.
- JavaScript interacts with the DOM to modify web pages dynamically.

### CSS & Styling
- **CSS (Cascading Style Sheets)** controls the appearance of elements.
- **CSS Inheritance**: Styles cascade from parent elements.

### JavaScript Fundamentals
- **Event-driven programming**: Scripts execute in response to user actions.
- **JSX**: JavaScript syntax extension for React.

---

## Programming Languages

### Domain-Specific Languages (DSL)
- **DSLs** are tailored for specific tasks, unlike general-purpose languages.

### Imperative vs. Declarative Languages
- **Imperative**: Explicitly defines steps to achieve a task (e.g., C, Python).
- **Declarative**: Describes what to achieve, letting the system determine how (e.g., SQL, HTML).

### Lisp Basics
- **Symbols & Conses**: Lisp structures programs using lists and recursive functions.

### Python History & Basics
- **Origins**: Python evolved from ABC, an educational language.
- **Basic Data Types**:
  - Lists (mutable), Tuples (immutable), Dictionaries (key-value mappings).

### Object-Oriented & Functional Programming
- **Object-Oriented**: Uses classes and objects.
- **Functional Programming**: Emphasizes pure functions and immutability.

### JavaScript & Node.js Event-driven Model
- **Node.js** executes JavaScript outside browsers.
- **Event Loop**: Handles asynchronous operations efficiently.

---

## Node.js & Asynchronous Programming

### Asynchronous Event Handling
- Uses an **event loop** to process non-blocking operations.

### Callbacks & Lambda Functions
- **Callbacks**: Functions passed as arguments to handle asynchronous results.
- **Lambdas**: Anonymous functions used in concise expressions.

### Node.js Package Management (npm)
- **npm (Node Package Manager)** manages dependencies.
- **package.json** stores project metadata and required libraries.

### Versioning (Semantic Versioning)
- **Major.Minor.Patch** (e.g., 2.1.3)
  - **Major**: Breaking changes.
  - **Minor**: Feature additions.
  - **Patch**: Bug fixes.

### Web Servers & Load Balancing in Node.js
- **Round-robin DNS** distributes traffic among multiple servers.
- **Scaling Techniques**:
  - Adding more processes.
  - Deploying additional servers.

### Parallelism vs. Concurrency in Event-driven Programming
- **Parallelism**: Multiple tasks execute simultaneously.
- **Concurrency**: Tasks run independently but not necessarily at the same time.

---

# Distributed Systems & Version Control

## Distributed Computing Principles

Distributed computing involves multiple computers working together over a network to achieve a common goal. These systems can be structured in various ways depending on the needs of the application.

### Key Concepts:
- **Decentralization**: No single point of failure; work is distributed across multiple nodes.
- **Concurrency**: Multiple computations happen simultaneously.
- **Scalability**: The system can grow and handle increased load by adding more nodes.
- **Fault Tolerance**: The system continues operating even if some nodes fail.

## Client-Server Model

The client-server model is the most common form of distributed computing.

### Characteristics:
- **Clients** request services.
- **Servers** process and fulfill requests.
- **Communication** occurs over a network, often using HTTP or TCP/IP.
- **Centralization**: The server acts as the single source of truth for data and state.

### Example:
A university’s enrollment system where students (clients) submit requests to enroll in a course, and the registrar’s office (server) processes them.

## Peer-to-Peer (P2P) Systems

Unlike the client-server model, P2P systems have no central authority.

### Characteristics:
- **Decentralized control**: No single point of failure.
- **Nodes** act as both clients and servers.
- **Scalability**: More peers can join without significantly affecting performance.
- **Examples**: BitTorrent, blockchain networks.

### Advantages:
- No reliance on a central server.
- Resilient against failure of individual nodes.
- Can handle large-scale distributed data storage and retrieval.

## Primary-Secondary Architecture

A system where one node (the primary) directs and controls other nodes (secondaries).

### Characteristics:
- **Primary node**: Assigns and manages tasks.
- **Secondary nodes**: Execute tasks assigned by the primary.
- **Example**: Database replication, where a primary database handles write operations while secondary databases handle read operations.

### Trade-offs:
- **High availability**: Secondary nodes can take over if the primary fails.
- **Scalability**: More secondaries improve read performance.
- **Complexity**: Requires coordination and failover mechanisms.

## Version Control Principles

Version control systems track changes to files, allowing multiple people to collaborate effectively and maintain historical changes.

### Git:
Git is the most widely used distributed version control system.

### Key Features:
- **Branching and Merging**: Allows parallel development.
- **Commit History**: Tracks all changes made over time.
- **Remote Repositories**: Supports collaboration across different machines.
- **Distributed**: Each user has a complete copy of the repository.

### Common Commands:
- `git init` – Initialize a new repository.
- `git clone <repo>` – Clone an existing repository.
- `git add <file>` – Stage changes.
- `git commit -m "message"` – Save changes.
- `git push` – Upload local changes to a remote repository.
- `git pull` – Fetch changes from a remote repository.
- `git merge <branch>` – Merge changes from another branch.

## Performance Trade-offs: Throughput vs. Latency

### Throughput
- Measures the amount of work the system can process per second.
- Important for servers handling many requests.
- **Example**: A web server processing 10,000 requests per second.

### Latency
- Measures the time taken for a single request to be processed.
- Critical for real-time applications.
- **Example**: The time it takes to fetch a webpage.

### Trade-offs
- **Increasing throughput can increase latency** if requests are processed in batches.
- **Reducing latency** may require more computational resources, limiting throughput.

## Caching and Data Consistency

Caching improves performance but introduces challenges in data consistency.

### Caching Strategies:
- **Client-side caching**: Stores frequently accessed data on the client.
- **Server-side caching**: Reduces database queries by storing responses temporarily.
- **CDN caching**: Caches static assets at edge locations.

### Issues:
- **Stale Data**: Cached data may become outdated.
- **Cache Invalidation**: Needs mechanisms to refresh cache when data changes.
- **Eviction Policies**: Determines how long data stays in cache (e.g., Least Recently Used - LRU).

### Solutions:
- **Timestamp-based validation**: Compare cache timestamps with server data.
- **ETags**: Use unique identifiers for cached data to determine if updates are needed.
- **Versioning**: Assign version numbers to cached data and update when necessary.

## Summary
- **Distributed systems** leverage multiple nodes to perform tasks.
- **Client-server models** are the most common, but **peer-to-peer** and **primary-secondary architectures** offer alternatives.
- **Version control (Git)** tracks changes and enables collaboration.
- **Performance optimization** requires balancing **throughput vs. latency**.
- **Caching** improves speed but must be managed carefully to ensure **data consistency**.

# Networking and Internet Protocols

## Packet Switching vs. Circuit Switching

### Circuit Switching
- Previously used in traditional telephone networks.
- Dedicated communication path established between two nodes before communication starts.
- Resources are reserved, ensuring guaranteed performance and a constant delay.
- Suitable for predictable, continuous data transmission.
- Inefficient when connections remain idle.

### Packet Switching
- Core concept of the Internet.
- Data is divided into small packets that travel independently through the network.
- More efficient than circuit switching as it maximizes network resource utilization.
- No reserved capacity, meaning congestion can introduce variable delays.
- Packets can be lost, reordered, or duplicated.

## Internet Protocol (IP, IPv4, IPv6)
- IP is responsible for addressing and routing packets across networks.
- **IPv4** (1983, designed by a UCLA alum, John Postel):
  - Uses 32-bit addresses (e.g., 192.168.1.1).
  - Connectionless protocol, meaning each packet is treated independently.
  - Contains a **TTL (Time To Live)** field that prevents infinite packet looping.
  - Includes a checksum for error detection.
- **IPv6** (1998):
  - Uses 128-bit addresses, accommodating a much larger address space.
  - Simpler header structure compared to IPv4.
  - Designed for future scalability and security.

## Transport Layer Protocols

### UDP (User Datagram Protocol)
- Lightweight and simple; just a thin layer over IP.
- Designed by David Reed at MIT.
- Does not guarantee packet order, reliability, or retransmission.
- Used for applications where speed matters more than reliability (e.g., gaming, VoIP, video streaming).

### TCP (Transmission Control Protocol)
- Designed by Vint Cerf (UCLA alum) and Bob Kahn.
- Reliable, ordered, and error-checked.
- Features:
  - **Flow control**: Ensures data is transmitted at a rate the receiver can handle.
  - **Retransmission**: Lost packets are resent.
  - **Reassembly**: Packets arrive in order at the destination.
  - Introduces overhead, making it slower than UDP.

### QUIC (Quick UDP Internet Connections)
- Developed by Google in 2012 to improve latency in Chrome.
- Built on top of UDP but adds reliability features similar to TCP.
- Reduces **head-of-line blocking**, an issue in TCP where packet loss affects the entire stream.

## Routing and Latency Concerns
- **Throughput**: Amount of useful work done per second.
- **Latency**: Time delay between sending a request and receiving a response.
- **Caching**: Helps reduce latency but can serve outdated data.
- **Serialization**: Ensuring operations occur in a logically valid order.

## Jitter and Real-Time Communication
- **Jitter**: Variability in packet arrival time.
- **RTP (Real-Time Transport Protocol)**:
  - Runs atop UDP to avoid TCP-induced jitter.
  - Designed for audio and video streaming.
  - Zoom and WebRTC rely on RTP to handle packet loss gracefully.

## Client-Server Communication
- The server is the ultimate arbitrator.
- Clients communicate through the server rather than directly with each other.
- Used in applications like web browsing, messaging, and online services.

### Peer-to-Peer (P2P) vs. Client-Server
- **Client-Server**: Centralized authority; more control but single point of failure.
- **P2P**: Decentralized; no single authority, harder to manage but more resilient.

## DNS Round-Robin Load Balancing
- **DNS resolves domain names to IP addresses.**
- Round-robin balancing helps distribute traffic by rotating among multiple IP addresses.
- Used to scale web services by spreading requests across multiple servers.

## Common Networking Issues
- **Packet loss**: Data packets fail to reach their destination due to congestion or hardware failure.
- **Out-of-order packets**: Packets arrive in an incorrect sequence, causing delays.
- **Duplication**: Misconfigured routers may send duplicate packets.
- **Network congestion**: Heavy traffic can slow down data transfer.

## Summary
The Internet operates based on packet-switching principles, with protocols like IP, TCP, UDP, and QUIC managing data transmission. Latency, jitter, and packet loss are common challenges, especially for real-time applications like video calls. The client-server model dominates web services, while P2P systems offer decentralized alternatives. Load balancing techniques like DNS round-robin help scale web applications efficiently.

# Web Development and Technologies Notes

## HTTP Versions

### HTTP/1.1
- Introduced multiple requests and multiple responses per connection.
- Improved performance by allowing parallelism between the client and server.
- Clients no longer had to wait for a request to complete before sending another.
- Helped reduce time spent in request-response cycles.

### HTTP/2
- Released in 2015, accounting for ~50% of web requests in 2024.
- Key improvements over HTTP/1.1:
  - **Pipelining:** Clients can send multiple requests before receiving responses.
  - **Multiplexing:** Allows multiple simultaneous conversations between browser tabs and the web server.
  - **Header Compression:** Uses gzip to compress headers, reducing size and improving efficiency.
  - **Server Push:** Allows servers to send multiple responses per request, reducing the need for clients to keep polling.

### HTTP/3
- Introduced in 2022, making up ~20% of web requests in 2024.
- Uses **UDP instead of TCP**, improving latency and performance.
- Implements **QUIC**, a transport protocol developed by Google in 2012.
- Designed to avoid "head-of-line" blocking issues inherent in TCP.
- Improved performance for real-time applications like **Zoom** and video streaming.

## Browser Rendering Pipeline and Optimizations

### Rendering Pipeline
- Browsers start rendering a page **before** it fully loads.
- Example: A browser starts displaying the `<html>`, `<head>`, and `<body>` elements incrementally.
- Rendering is done **before the entire page is downloaded**, making the user experience smoother.

### Optimizations
- Elements not on the screen **are not processed** until needed.
- Scripts that are not visible **may not execute**.
- Browsers **guess** the layout in advance and update it as needed.
- Example: **Table Rendering**
  - Tables often do not have predefined dimensions.
  - The browser will guess and resize it dynamically as content is loaded.

## HTML Structure and Syntax

- Based on **SGML (Standard Generalized Markup Language)**.
- HTML **focuses on content**, separating it from styling (handled by CSS).
- Uses **elements** with **tags** to structure content.
  - `<html>`: Root element
  - `<head>`: Metadata
  - `<body>`: Content

### Tags
- Opening and closing tags are used to define elements.
- Some tags can self-close, such as:
  ```html
  <br/>
  ```
- Attributes provide **additional information** about elements:
  ```html
  <img src="image.jpg" alt="Description">
  ```

## XML vs. JSON for Data Representation

### XML (Extensible Markup Language)
- Hierarchical **tree structure**.
- Originally designed for **document exchange**.
- Example:
  ```xml
  <book>
    <title>Introduction to CS</title>
    <author>John Doe</author>
  </book>
  ```
- More verbose compared to JSON.

### JSON (JavaScript Object Notation)
- **Simpler** and more lightweight than XML.
- A subset of JavaScript but widely used in **API communication**.
- Example:
  ```json
  {
    "book": {
      "title": "Introduction to CS",
      "author": "John Doe"
    }
  }
  ```
- JSON is the **preferred format** for most web applications due to its compactness and ease of parsing.

## Document Object Model (DOM)
- **Tree representation** of an HTML document.
- Allows JavaScript to **access and modify** webpage content dynamically.
- Example of DOM manipulation:
  ```javascript
  document.getElementById("title").innerText = "New Title";
  ```
- **APIs for DOM:**
  - **Traversing**: Navigate elements within the document tree.
  - **Updating**: Modify element properties dynamically.
  - **Styling**: Apply CSS properties programmatically.

## CSS and Cascading Style Rules

### Cascading Style Sheets (CSS)
- Defines how HTML elements **appear on screen**.
- Separates **style from content**, supporting the **declarative programming model** of SGML.

### Style Inheritance
- Styles **cascade** from **parent elements**.
- Priorities:
  1. **Inline styles** (highest priority)
  2. **Author-defined stylesheets**
  3. **User-defined styles**
  4. **Browser defaults**
- Example:
  ```html
  <span style="color: blue;">Blue Text</span>
  ```

## JavaScript Fundamentals

### JavaScript Basics
- JavaScript is **event-driven**, often modifying the DOM dynamically.
- Example of an inline script:
  ```html
  <script>
    alert("Hello, JavaScript!");
  </script>
  ```
- Scripts can also be included externally:
  ```html
  <script src="script.js"></script>
  ```

## Event-Driven Programming in JavaScript

### Event Handling Model
- JavaScript programs **react to user interactions**.
- Event-driven programs **do not control execution directly** but respond to triggered events.
- Example of an event listener:
  ```javascript
  document.getElementById("btn").addEventListener("click", function() {
    alert("Button clicked!");
  });
  ```

### Node.js and Asynchronous Execution
- Node.js **follows an event-driven architecture**.
- Uses **callbacks** to handle asynchronous events efficiently.
- Example of an event loop:
  ```javascript
  while (true) {
    let e = getNextEvent();
    handleEvent(e);
  }
  ```
- Asynchronous programming avoids **blocking operations**, allowing applications to remain responsive.

---

# Programming Languages Notes

## Domain-Specific Languages (DSL)
- A domain-specific language is designed for a specific application domain.
- It works well within its domain but does not generalize well to other areas.
- Examples:
  - SQL (structured query language) for databases.
  - Regular expressions for pattern matching.
  - HTML/CSS for web page styling and layout.
  
## Imperative vs. Declarative Languages
- **Imperative programming**: Describes how a program operates.
  - Uses statements to change a program's state.
  - Example: C, Python, Java.
- **Declarative programming**: Describes what the program should accomplish.
  - Focuses on logic rather than control flow.
  - Example: SQL, HTML, functional languages like Haskell.
  
## Lisp Basics
- One of the oldest programming languages still in use.
- **Atomic values**: Integer, string, symbols.
- **Cons cells**: The basic data structure in Lisp.
  - `cons (expr1 expr2)`: Creates a pair of two objects.
  - Printed as `(expr1 . expr2)`.
- **Lists**: Built using cons cells.
  - `(A (B (C nil)))`.
  - `nil` represents an empty list.
- **Basic functions**:
  - `car (expr)`: Returns the first element of a list.
  - `cdr (expr)`: Returns the rest of the list after the first element.
  - `quote` or `'` prevents evaluation of expressions.
  - `(setq x value)`: Assigns `value` to `x`.
  - `(defun f (a b c) (code))`: Defines a function.
  
## Python History and Basics
- **Origins**:
  - Evolved from ABC, a language intended for teaching.
  - ABC emphasized indentation and built-in data structures.
  - Guido van Rossum created Python to improve upon ABC.
- **Core principles**:
  - Indentation matters; no `{}` for block definition.
  - Every value is an object with:
    - An **identity** (`id(obj)`).
    - A **type** (`type(obj)`).
    - A **value** (`str(27) → '27'`).
- **Data types**:
  - `NoneType`: `None`
  - Numbers: `int`, `float`
  - Sequences: `list`, `tuple`, `str`
  - Mapping: `dict`
- **Mutable vs. Immutable**:
  - Lists are **mutable** (`s[i] = value`).
  - Tuples are **immutable** (`(5, 6, 7)`).
  
## Object-Oriented Programming in Python
- Everything in Python is an **object**.
- Attributes: `o.a`
- Methods: `o.m(args...)`
- **Comparison**:
  - `a is b`: Identity comparison (same object).
  - `a == b`: Value comparison.
- **List operations**:
  - `s.append(value)`: O(1) amortized.
  - `s[i:j] = new_list`: Slice assignment.
  - `del s[i]`: Removes an item.
  
## Functional Programming Concepts
- **First-class functions**: Functions can be assigned to variables and passed as arguments.
- **Map, Filter, Reduce**:
  - `map(func, seq)`: Applies `func` to each item.
  - `filter(func, seq)`: Filters items where `func(item) == True`.
  - `reduce(func, seq)`: Reduces sequence to a single value.
- **Lambda functions**:
  - Anonymous, short functions: `lambda x: x + 2`.
- **List comprehensions**:
  - `[x for x in seq if condition]`.
- **Closures**:
  - Functions that capture variables from their enclosing scope.
  - Example:
    ```python
    def make_multiplier(n):
        return lambda x: x * n
    double = make_multiplier(2)
    double(4)  # Output: 8
    ```
  
## JavaScript and Node.js Event-Driven Model
### JavaScript Overview
- **Origins**:
  - Originally an extension language for browsers.
  - Dynamically typed, interpreted language.
- **Core Features**:
  - Prototypal inheritance.
  - Asynchronous programming (callbacks, promises, async/await).
  - DOM manipulation.
- **Interaction with HTML**:
  - `<script>` tags execute JavaScript in browsers.
  - JS can modify the DOM dynamically.

### Node.js Overview
- **What is it?**
  - A runtime for executing JavaScript outside the browser.
  - Built on Chrome's V8 JavaScript engine.
  - Uses an **event-driven**, **non-blocking** I/O model.
- **Event Loop**:
  - Instead of running multiple threads, Node.js processes events asynchronously.
  - Single-threaded but can handle many I/O operations simultaneously.
- **Example Event Loop Code**:
  ```javascript
  while (true) {
      let e = getNextEvent();
      handleEvent(e);
  }
  ```
- **Advantages**:
  - Avoids multi-threading issues (e.g., race conditions).
  - Good for I/O-heavy applications.
  - Scales well by adding more servers rather than cores.
- **Creating a Simple Web Server**:
  ```javascript
  const http = require('http');
  const server = http.createServer((req, res) => {
      res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
      res.end('Hello, world!');
  });
  server.listen(3000, '127.0.0.1', () => {
      console.log('Server running at http://127.0.0.1:3000/');
  });
  ```
- **NPM (Node Package Manager)**:
  - Handles package dependencies.
  - `package.json` stores project dependencies.
  - Commands:
    - `npm init`: Initializes a new project.
    - `npm install module`: Installs a module.
    - `npm install --save module`: Saves dependency in `package.json`.
  
### Event Handling vs. Multi-threading
- **Multi-threading**: Uses multiple CPUs but introduces race conditions.
- **Event handling**: More reliable, avoids race conditions.
- **Scaling strategies**:
  - Scale by adding more cores.
  - Scale horizontally by adding more servers.

---

# Node.js and Asynchronous Programming

## Asynchronous Event Handling

Node.js follows an **event-driven programming model**, meaning execution is driven by external events rather than a sequential flow of control. The Node.js runtime continuously listens for events and triggers appropriate event handlers.

### Event Loop:

```javascript
while (true) {
    e = getNextEvent();
    handleEvent(e);
}
```

- This structure ensures that the system is always ready to handle new events as they occur.
- The **event loop** continuously listens for new events and executes the relevant handler function.
- Execution happens in non-blocking, asynchronous fashion.

### Key Aspects:
- Each handler runs to completion before another event is processed.
- Event handlers should execute quickly to avoid blocking the event loop.
- Node.js is single-threaded, making it highly efficient for I/O-bound operations but less suited for CPU-intensive tasks.

## Callbacks and Lambda Functions

### Callbacks:
- Callbacks are functions passed as arguments to be executed later when an asynchronous operation completes.

Example:

```javascript
const fs = require('fs');
fs.readFile('file.txt', 'utf8', (err, data) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log(data);
});
```

### Lambda Functions (Arrow Functions):
- Lambda functions provide a concise syntax for defining functions.

Example:

```javascript
const add = (a, b) => a + b;
console.log(add(2, 3)); // Outputs: 5
```

- Frequently used as callback functions due to their compact syntax.

## Node.js Package Management

### npm (Node Package Manager)
- **npm** is the default package manager for Node.js, used to install, update, and manage dependencies.

#### Important npm Commands:

```sh
npm init      # Initialize a new project with a package.json file
npm install <module>    # Install a package locally
npm install --save <module>  # Add a package as a dependency and update package.json
npm install    # Install all dependencies listed in package.json
```

- `package.json` is the central metadata file for a Node.js project.
- It stores:
  - Project name, version, and description
  - List of dependencies and devDependencies
  - Scripts for automating tasks

Example `package.json`:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

## Versioning in Node.js (Semantic Versioning)

Node.js and its ecosystem follow **semantic versioning** to manage updates.

Versioning follows this pattern: `MAJOR.MINOR.PATCH`

- **PATCH** (e.g., `5.2.7` → `5.2.8`): Fixes bugs, maintains backward compatibility.
- **MINOR** (e.g., `5.2.7` → `5.3.0`): Adds new features without breaking existing functionality.
- **MAJOR** (e.g., `5.2.7` → `6.0.0`): Introduces breaking changes that may require code modifications.

Understanding versioning is crucial when managing dependencies to prevent breaking changes in your project.

## Web Servers and Load Balancing in Node.js

### Creating a Simple Web Server:

```javascript
const http = require('http');
const ipaddr = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello, this is a simple server!');
});

server.listen(port, ipaddr, () => {
    console.log(`Server running at http://${ipaddr}:${port}/`);
});
```

### Load Balancing:

- To handle high traffic, multiple instances of a server can be run.
- **Round Robin DNS**: Different IP addresses returned in response to each request.
- **Reverse Proxy (e.g., Nginx, HAProxy)**: Distributes requests among multiple servers.
- **Clustering**: Using Node.js `cluster` module to spawn multiple processes.

```javascript
const cluster = require('cluster');
const os = require('os');
const http = require('http');

if (cluster.isMaster) {
    const numCPUs = os.cpus().length;
    for (let i = 0; i < numCPUs; i++) {
        cluster.fork();
    }
} else {
    http.createServer((req, res) => {
        res.writeHead(200);
        res.end('Hello from worker process');
    }).listen(3000);
}
```

## Parallelism vs. Concurrency in Event-Driven Programming

- **Concurrency**: Multiple tasks make progress but are not necessarily executed at the same time.
- **Parallelism**: Multiple tasks execute simultaneously on multiple CPUs/cores.
- **Node.js is single-threaded**, meaning it can handle concurrent operations (e.g., handling multiple I/O requests) efficiently but does not execute them in parallel.
- For **CPU-intensive tasks**, Node.js can spawn worker threads or delegate work to external processes.

### Summary

| Concept | Description |
|---------|-------------|
| **Event Loop** | Continuously listens and processes incoming events asynchronously. |
| **Callbacks & Lambdas** | Functions passed to handle events and executed later. |
| **npm & package.json** | Manages dependencies and project metadata. |
| **Semantic Versioning** | Defines update types as PATCH, MINOR, and MAJOR. |
| **Web Server** | Built-in HTTP module for handling requests. |
| **Load Balancing** | Distributes traffic among multiple instances using clustering, proxies, and DNS techniques. |
| **Concurrency vs. Parallelism** | Node.js handles I/O-bound tasks efficiently but requires workarounds for CPU-heavy tasks. |

This document provides an overview of key concepts in Node.js related to asynchronous programming, package management, web server setup, and scaling strategies.

# Regular Expressions (Regex) in the Linux Terminal

Regular expressions (regex) are powerful tools for pattern matching and text processing in the Linux terminal. They are widely used with commands like `grep`, `sed`, `awk`, and others.

## 📌 Basics of Regular Expressions

A **regular expression** is a sequence of characters that define a search pattern. In Linux, regex is commonly used for:

- Searching for specific text in files (`grep`)
- Modifying text (`sed`)
- Text processing (`awk`)

## 🎯 Types of Regular Expressions

1. **Basic Regular Expressions (BRE)** – Used by `grep` by default.
2. **Extended Regular Expressions (ERE)** – Used by `grep -E`, `sed -E`, and `awk`.

---

# 🔍 `grep` vs. `grep -E`

### `grep` (Basic Regular Expressions - BRE)
- **Uses basic regex syntax**.
- Some special characters need escaping (`\`).

### `grep -E` (Extended Regular Expressions - ERE)
- **Supports extended syntax**.
- No need to escape certain meta-characters.

#### 🔹 Example:

```bash
echo "apple banana cherry" | grep "apple\|banana"
# No match, because | needs escaping in BRE

echo "apple banana cherry" | grep -E "apple|banana"
# Matches "apple" and "banana"
```

---

# 🛠️ Key Regex Components

## 1️⃣ **Literals and Meta-Characters**
| Symbol | Description | Example |
|---------|------------|---------|
| `.` | Any single character | `c.t` → Matches `cat`, `cot`, `cut` |
| `*` | Zero or more of the preceding character | `a*` → Matches `aaa`, `a`, or nothing |
| `+` | One or more of the preceding character (**ERE only**) | `ba+` → Matches `ba`, `baa`, `baaa` |
| `?` | Zero or one occurrence (**ERE only**) | `colou?r` → Matches `color` or `colour` |
| `|` | OR operator (**ERE only**) | `apple|banana` matches either |
| `^` | Anchors match at the start of the line | `^hello` → Matches lines starting with `hello` |
| `$` | Anchors match at the end of the line | `world$` → Matches lines ending with `world` |

## 2️⃣ **Character Classes**
| Expression | Description | Example |
|------------|------------|---------|
| `[abc]` | Any one of `a`, `b`, or `c` | `gr[ae]y` → Matches `gray` or `grey` |
| `[^abc]` | Any character **except** `a`, `b`, or `c` | `[^0-9]` → Matches non-digit characters |
| `[0-9]` | Any digit (same as `\d` in some tools) | `[A-Z]` → Matches uppercase letters |
| `[a-zA-Z0-9]` | Any alphanumeric character | Matches `abc123XYZ` |

## 3️⃣ **Predefined Character Classes**
| Pattern | Meaning | Example |
|---------|---------|---------|
| `\d` | Any digit (0-9) (**ERE only**) | `\d+` → Matches `123`, `42` |
| `\w` | Word character (alphanumeric + `_`) | `\w+` → Matches `hello_42` |
| `\s` | Any whitespace (space, tab, newline) | `\s+` → Matches spaces |

---

# 🔥 Advanced Regex Examples

### 1️⃣ **Find all email addresses**
```bash
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt
```
**Explanation**: This regex looks for patterns matching an email format: a combination of letters, numbers, and special characters (`_ . % + -`) before `@`, followed by a domain and a valid TLD (`.com`, `.org`, etc.).

### 2️⃣ **Extract valid IPv4 addresses**
```bash
grep -E "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" file.txt
```
**Explanation**: This pattern matches four groups of numbers (0-255) separated by dots (`.`), ensuring valid IPv4 format.

### 3️⃣ **Find lines containing numbers at the beginning**
```bash
grep -E "^[0-9]+" file.txt
```
**Explanation**: The `^` ensures the match starts at the beginning of the line, followed by one or more digits.

### 4️⃣ **Find lines ending with a question mark**
```bash
grep -E "\?$" file.txt
```
**Explanation**: The `\?` matches a literal question mark, and `$` ensures it appears at the end of the line.

### 5️⃣ **Find words that repeat twice in a row**
```bash
grep -E "\b([a-zA-Z]+) \1\b" file.txt
```
**Explanation**: This captures a word (`([a-zA-Z]+)`) and ensures the same word appears again (`\1`). The `\b` ensures full word matches (e.g., `hello hello` but not `helloohello`).

---

## 🚀 Summary
- **`grep`** → Uses basic regex (BRE), requiring extra escaping.
- **`grep -E`** → Uses extended regex (ERE), simplifying complex patterns.
- **Use metacharacters (`. * + ? ^ $ [] {}`) to construct patterns.**
- **Regex is powerful for text search, validation, and transformation.**

---






