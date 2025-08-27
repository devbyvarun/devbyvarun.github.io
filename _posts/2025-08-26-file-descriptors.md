---
title: "Understanding File Descriptors"
date: 2025-08-26 12:00:00 -0700
categories: [System, Unix]
tags: [file descriptors, unix, i/o, resources]
description: "An overview of what file descriptors are, how they work in Unix-like systems, and why they matter."
---

## What Is a File Descriptor?

In Unix-like operating systems, a **file descriptor (FD)** is an integer that uniquely identifies an open file (or I/O stream) for a process.

## Common Uses

File descriptors are used to interact with:

- Regular files  
- **Standard streams**:  
  | FD | Purpose              |
  |----|----------------------|
  | 0  | Standard input       |
  | 1  | Standard output      |
  | 2  | Standard error       |

## How It Works

When your program opens a file—say, with `open("example.txt", O_RDONLY)`—the OS returns an FD, such as `3`, which your program then uses to `read(3, ...)`, `write(3, ...)`, or `close(3)`.

## Code Example

```c
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd = open("data.txt", O_RDONLY);
    if (fd == -1) {
        perror("open");
        return 1;
    }
    char buffer[100];
    ssize_t bytes = read(fd, buffer, sizeof(buffer) - 1);
    if (bytes > 0) {
        buffer[bytes] = '\\0';
        write(1, buffer, bytes);  # Write to stdout
    }
    close(fd);
    return 0;
}
