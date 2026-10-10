# Reflection

## The topic

I chose to visualize stack-based buffer overflow. The core idea I wanted to make clear is that a fixed-size buffer like `char buf[64]` has no bound checking in `strcpy`, so input longer than the buffer keeps writing into adjacent stack memory — first the saved frame pointer (EBP), then the saved return address (EIP). Once EIP is controlled, the attacker controls where the program jumps when the function returns.

## What I built

I built a single HTML/JavaScript file that renders the stack as labeled regions and lets the user fill the buffer interactively with a text box and a slider. I added preset buttons for the key moments (buffer full at 64 bytes, into the saved frame pointer at 72, the segfault point at 76, and the NOP-sled exploit layout), a register readout that shows the resulting EBP and EIP values, and a verdict panel that updates as the overflow grows.

## My workflow

- I started from the lab's `vuln.c` and the compile flags to anchor the correct offsets: a 64-byte buffer, 4-byte alignment, EBP at bytes 65–68, and EIP at 69–72.
- I used AI to scaffold the byte-grid layout and draft the explanation text, then reviewed it and tested it in the browser.
- I iterated by checking that the byte counts lined up with the real offsets and adjusting the presets and wording until the overflow behaved like the lab setup.

## How it deepened my understanding

Building the visualization forced me to be precise about ordering and offsets in a way that just reading about overflows did not. I understood much more clearly why the saved EBP sits between the buffer and the return address, and why a segmentation fault is actually the useful signal — it means EIP now holds my bytes and I control the jump target.

Mapping the concept to the exact bytes also made Problem 2 easier to follow, because I already knew what I expected to see in `info registers` and in the `x/80bx $esp` stack dump before I ran gdb.

## What I'd improve

I'd add an animation showing execution sliding down the NOP sled into the shellcode.
