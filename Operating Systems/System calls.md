# What are they

# Types of `syscalls`
- Process control
- File Management
- Device Management
- Information Maintenance
- Communications
## Examples
Types of Sys Calls|Windows|Linux
:--:|:--:|:--:
Process Control|CreateProcess()|fork()
-|ExitProcess()|exit()
-|WaitForSingleObject()|wait()
-| |
File Management|CreateFile()|open()
-|ReadFile()|read()
-|WriteFile()|write()
-|CloseHandle()|close()
-| |
Device Management|SetConsoleMode()|ioctl()
-|ReadConsole()|read()
-|WriteConsole()|write()
-| |
Information Maintenance|GetCurrentProcessID()|getpid()|
-|SetTimer()|alarm()|
-|Sleep()|sleep()|
-| |
Communication|CreatePipe()|pipe()|
-|CreateFileMapping()|shmget()|
-|MapViewOfFile()|mmap()|
