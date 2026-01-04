# Linux Kernel Module – Hello World (Deep Dive)

This repository documents the development of a minimal **Linux Loadable Kernel Module (LKM)** with the goal of gaining a *clear, conceptual, and practical understanding* of how Linux kernel modules are built, loaded, executed, and unloaded at runtime.

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

.
├── hello.c # Kernel module source code
├── Makefile # Kernel module Makefile
├── .gitignore # Excludes generated build artifacts
└── README.md # Project documentation


---

## Source Code Overview

### `hello.c`

The module implements:
- An initialization function that runs when the module is loaded
- A cleanup function that runs when the module is removed
- Kernel log messages to verify execution

Key elements:
- `module_init()` and `module_exit()` macros
- `__init` and `__exit` annotations
- `printk()` for kernel-space logging
- Module metadata such as license and author

---

## Build Instructions

Install the required development tools and kernel headers:

```bash
sudo apt install build-essential linux-headers-$(uname -r)'''
Compile the kernel module:

make


This produces the loadable kernel object:

hello.ko


The module is compiled using the kernel’s own build system, ensuring:

ABI compatibility

Correct compiler flags

Symbol version checking

Module Insertion and Verification

Insert the module into the running kernel:

sudo insmod hello.ko


Verify execution by checking kernel logs:

sudo dmesg | tail


Expected output:

Hello, kernel module loaded


This confirms that:

The module was successfully loaded

The initialization function executed inside kernel space

Module Removal and Cleanup

Remove the module:

sudo rmmod hello


Verify cleanup:

sudo dmesg | tail


Expected output:

Goodbye, kernel module unloaded


This confirms:

Cleanup logic executed correctly

The module was safely removed

No kernel crash or resource leak occurred

Secure Boot and Kernel Tainting Notes

Secure Boot may block unsigned kernel modules

On development systems, Secure Boot can be disabled or module signing configured

Loading an out-of-tree module taints the kernel

Kernel tainting is expected and acceptable during development

Learning Outcomes

By completing this project, the following skills were strengthened:

Practical understanding of Linux kernel module lifecycle

Confidence in building and loading kernel code

Understanding of kernel-space execution and logging

Familiarity with Secure Boot and kernel security mechanisms

Readiness to move into Linux device driver development
