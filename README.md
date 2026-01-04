# Linux Kernel Module – Hello World

This repository documents the development of a minimal **Linux Loadable Kernel Module (LKM)** with the goal of gaining a **clear, conceptual, and practical understanding** of how Linux kernel modules are built, loaded, executed, and unloaded at runtime.

Rather than treating kernel programming as a black box, this project focuses on **why each step exists**, how the **kernel build system works**, and how **user actions trigger kernel-level execution**.

This project serves as a **foundational step toward Linux device driver development**.

---

## Motivation

Modern embedded and system software roles require more than surface-level Linux knowledge.  
Understanding **how code runs inside the kernel** is essential for:

- Device driver development  
- Embedded Linux and BSP work  
- Kernel debugging and performance analysis  
- Understanding OS internals beyond user space APIs  

This project was created to:
- Move beyond theory  
- Gain hands-on experience with kernel internals  
- Build confidence in Linux kernel programming fundamentals  

---

## What Is a Kernel Module?

A **kernel module** is a piece of code that can be:

- Compiled separately from the kernel  
- Loaded dynamically into a running kernel  
- Removed safely without rebooting the system  

Unlike user-space programs:

- Kernel modules run in **kernel space**  
- They share memory with the kernel  
- Errors can crash the entire system if not handled carefully  

This makes correctness, discipline, and understanding critical.

---

## Objectives

The primary objectives of this project are:

- Understand the lifecycle of a Linux kernel module  
- Learn how the kernel build system compiles external modules  
- Implement proper initialization and cleanup routines  
- Execute and verify kernel-space code using kernel logs  
- Understand Secure Boot, unsigned modules, and kernel tainting  
- Build a solid base for writing Linux character device drivers  

---

## Key Concepts Explored

### Kernel Space vs User Space
- Kernel modules execute in kernel space with full hardware access  
- Standard C libraries and system calls are not available  
- Kernel-specific APIs must be used  

### Loadable Kernel Modules (LKM)
- Modules are inserted using `insmod`  
- Removed using `rmmod`  
- Managed by the kernel module loader  

### Module Lifecycle
- `__init` functions run only during module insertion  
- `__exit` functions run during module removal  
- Initialization memory can be freed after use  

### Kernel Logging
- `printk()` is used instead of `printf()`  
- Logs are stored in the kernel ring buffer  
- Logs are accessed using `dmesg`  

### Kernel Build System
- Modules are compiled using the kernel’s own build system  
- `obj-m` is used to declare loadable modules  
- ABI and symbol compatibility are enforced automatically  

### Secure Boot and Kernel Tainting
- Unsigned modules may be rejected if Secure Boot is enabled  
- Loading out-of-tree modules taints the kernel  
- Kernel tainting is expected during development  

---

## Build Environment

- **Operating System:** Ubuntu Linux  
- **Kernel Version:** 6.14.0-36-generic  
- **Architecture:** x86_64  
- **Compiler:** GCC 13  
- **Build System:** Linux Kernel Build System  
- **Module Type:** Out-of-tree loadable kernel module  

---

## Project Structure

```text
.
├── hello.c        # Kernel module source code
├── Makefile       # Kernel module Makefile
├── .gitignore     # Excludes generated build artifacts
└── README.md      # Project documentation
