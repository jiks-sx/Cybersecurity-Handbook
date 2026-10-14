# 1. Introduction to Computers

## What is a computer?

A computer is an electronic machine that receives information, follows instructions to work on it, and gives a result.

For example, when you type `2 + 3` into a calculator, the computer receives your input, performs the calculation, and shows `5`. It can also save photos, play videos, open websites, and send data to other computers.

A simple model is:

```text
Input -> Processing -> Output
             |
           Memory

Storage keeps files and programs for later.
```

- **Input:** information the computer receives, such as a key press or network message.
- **Processing:** work done according to instructions.
- **Output:** the result, such as text on a screen or data sent over a network.
- **Memory:** information being used right now.
- **Storage:** information kept for later, even after shutdown.

## Hardware, software, and firmware

### Hardware

Hardware is the physical part of a computer. Examples include the CPU, RAM, SSD, keyboard, screen, and network adapter.

### Software

Software is a set of instructions that tells a computer what work to do. Examples include Windows, Linux, a browser, Python, and security tools.

### Firmware

Firmware is low-level software that helps a device start or control basic functions. UEFI firmware is one example.

## How they work together

When I open a browser, the browser is software. The CPU runs its instructions, RAM holds information it is using, and the operating system helps it access files, the screen, and the network. Firmware helped the computer start.

## Why this matters in cybersecurity

Security problems can happen at different levels. An unsafe application, an outdated driver, and a weak firmware setup are different problems and may need different fixes.

## Quick check

1. Give two examples of hardware.
2. What is one difference between software and firmware?
3. When you open a browser, which parts of the computer help it run?

## Behind the scenes: opening a website

When you open a website, several parts work together:

1. You enter a URL in the browser.
2. The browser asks the operating system to use the network.
3. The operating system and network adapter send and receive data.
4. The CPU runs browser and operating-system instructions.
5. RAM holds active page data and program state.
6. The browser interprets the response and draws the page on the screen.

This is a simplified flow. DNS, TCP or QUIC, TLS, HTTP, caches, and server-side systems may also be involved. Those topics are covered in the networking and web-security chapters.

## Real-world example: a slow computer

A slow computer does not automatically mean it has malware. Possible causes include:

- Too many programs competing for CPU time.
- Low available RAM and frequent paging.
- A storage device with high latency or little free space.
- A background update or backup.
- A browser tab using a lot of resources.
- A failing device or a software bug.

A good technician checks evidence before choosing a fix. For example, `top` can help check CPU and memory usage, while `df -h` shows filesystem space.

## Questions to ask when troubleshooting

- Did the issue start after a change?
- Is one program slow, or is the whole computer slow?
- Does the issue continue after a restart?
- Are there useful logs or error messages?
- Can I reproduce the issue safely?

## Common misunderstanding

A computer is not just the CPU. The CPU, memory, storage, operating system, applications, firmware, and devices work together. A problem in one area can affect the whole experience.
