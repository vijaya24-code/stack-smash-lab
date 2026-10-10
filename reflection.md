# Reflection

> This is a template in your own voice. Fill the **[brackets]** with what you actually did, and delete any line that doesn't fit. Keep it to roughly one page. Do not submit the brackets.

## The topic

I chose to visualize **stack-based buffer overflow**. The core idea I wanted to make clear is that a fixed-size buffer like `char buf[64]` has no bound checking in `strcpy`, so input longer than the buffer keeps writing into adjacent stack memory — first the saved frame pointer (EBP), then the saved return address (EIP). Once EIP is controlled, the attacker controls where the program jumps when the function returns.

## What I built

I built a single HTML/JavaScript file that renders the stack as labeled regions and lets the user fill the buffer interactively with a text box and a slider. [Add anything you changed or tuned yourself — e.g. "I adjusted the preset values" / "I added the register readout" / "I reworded the explanations."]

## My workflow

[Describe how you actually worked. Example skeleton:]
- I started from the lab's `vuln.c` and the compile flags to anchor the correct offsets (64-byte buffer, 4-byte alignment, EBP at bytes 65–68, EIP at 69–72).
- I used AI to [generate the initial layout / scaffold the byte grid / write the explanation text], then [reviewed / edited / tested] it in the browser.
- I iterated by [what you changed after seeing it run].

## How it deepened my understanding

Building the visualization forced me to be precise about **ordering and offsets** in a way that just reading about overflows did not. [Add one concrete thing you understood better — e.g. why the saved EBP sits between the buffer and the return address, why the overwrite happens in that specific order, why a segfault is actually the signal that EIP is controlled, or how little-endian byte order affects how the return address is written.]

Mapping the concept to the exact bytes also made Problem 2 easier to follow, because I already knew what I expected to see in `info registers` and in the `x/80bx $esp` stack dump before I ran gdb.

## What I'd improve

[Optional — one honest line. e.g. "I'd add an animation of the sled-and-jump" or "I'd show the little-endian conversion step explicitly."]
