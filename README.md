# Stack Smash Lab

An interactive, browser-based visualization that teaches **stack-based buffer overflow** — built for SE/CprE 4210 Learning Sprint #2, Problem 1.

Drop any input into a simulated 64-byte buffer and watch the bytes climb past the end of the buffer and overwrite the **saved frame pointer (EBP)** and the **saved return address (EIP)** — the exact corruption behind a classic stack smash.

## What it does

- A live stack diagram with three regions: `buf[64]`, saved EBP (4 bytes), and saved EIP (4 bytes).
- **Type** into the input box or **drag a slider** to fill the buffer with `A` characters and watch the overflow happen byte by byte.
- **Preset buttons** jump to the key moments: buffer full (64), into the SFP (72), the segfault point (76), and the shape of a NOP-sled + shellcode exploit payload.
- A **register readout** showing the resulting saved EBP / EIP values (e.g. `0x41414141` when overwritten with `A`s).
- A **verdict panel** that explains, at each fill level, whether the input is safe, has corrupted the frame pointer, or has fully hijacked the return address.

The layout and offsets match the Problem 2 lab exactly: a 64-byte buffer, 4-byte alignment (`-mpreferred-stack-boundary=2`), no stack canary (`-fno-stack-protector`), and an executable stack (`-z execstack`). It uses the same `0xDEADBEEF` / `0xCAFEBABE` marker convention the lab uses to locate EBP and EIP.

## Why a segfault matters

When the saved return address is overwritten with `0x41414141` ("AAAA"), the CPU jumps there on `ret` and crashes with **Segmentation fault (core dumped)**. That crash is the goal — it proves you now control EIP, which the real exploit redirects into a NOP sled leading to shellcode.

## Running it

It's a single self-contained HTML file. No build, no dependencies.

```
open index.html        # macOS
xdg-open index.html    # Linux
# or just double-click it
```

Any modern browser works.

## Educational scope

This is a **teaching tool**, not an attack tool. It contains no real shellcode, no exploit bytes, and does not generate a working payload. The "exploit layout" preset shows the *shape* of a NOP sled + shellcode + redirected return address for instruction only — the actual shellcode in the lab comes from the course-provided `shellcode.asm`.

## Files

- `index.html` — the visualization (single file, open in a browser)
- `reflection.md` — build reflection
- `README.md` — this file

## Built with

AI-assisted development (Claude). See `reflection.md` for the workflow.
