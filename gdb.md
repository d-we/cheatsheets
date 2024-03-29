# gdb Cheatsheet

## Open gdb
- Start gdb with binary: `gdb <binary>`
- Start gdb with binary and args: `gdb --args <binary> <args>`
- Attach gdb to process: `gdb -p <pid>`
- Open coredump: `gdb <binary> <coredump of binary>`

## Handling Execution
- Start execution: `run`
- Start execution and break at start: `start`

- Breakpoint at address: `break *<addr>`
- Breakpoint at function: `break <fun-name>`
- Breakpoint at source line: `break <file>:<line-number>`
- List breakpoints: `info breakpoints`
- Delete breakpoint: `delete <breakpoint-no>`

- Run until next breakpoint: `continue`
- Run until return of function: `finish`
- Run program until condition is false: `watch <condition>`
- Run program until address is read: `rwatch *<addr>`
- Run program until address is written: `watch *<addr>`
- Run program until address is read or written: `awatch *<addr>`
- Step to next source-code instruction: `step`
- Step to next assembly instruction: `stepi` or `si`
- Step-over (source-line): `next`
- Step-over (instruction): `nexti`

## Altering Execution
- Set register to a certain value: `set <reg>=<value>`, e.g., `set $rax=1337`
## Concurrent Execution
- List threads: `info threads`
- Follow thread: `thread <no>`
- Configure fork behavior: `set follow-fork-mode <child|parent>`

## Memory
- Print variable content: `print <var-name>`
- Print memory: `x/<num-bytes><unit><format> <addr>`
  * `<unit>`:
    * b: Byte
    * h: Half-word (2 bytes)
    * w: Word (4 bytes)
    * g: Giant (8 bytes)
  * `<format>`: 
    * s: C-string
    * c: character
    * i: instruction
    * d: decimal
    * x: hexa-decimal
    * b: binary
  * e.g. `x/gx $rdi`
- Dump memory to a file: `dump binary memory <out-filename> <memory-addr-start> <memory-addr-end>`
  * note that the end address is exclusive
- Show memory pages (gef-specific): `vmmap`

## Gather Information
- Show disassembly: `disassemble`
- Show disassembly for function/address: `disassemble <fun-name/addr>`
- Show functions: `info functions`
- Show source-code: `list <filename>:<fun-name/line-number>`

## Miscellaneous
- Handling signals: `handle <signal> <options>`
  * `<options>`:
    * `(no)print`: (Don't) print a message upon catching signal
    * `(no)stop`: (Don't) stop the program upon catching signal
    * `(no)pass`: (Don't) pass the signal to debuggee upon catching signal
  * e.g. `handle SIGSEGV nostop noprint pass` (useful for debugging Java programs)

## gef-specific
- Heap-base: `p $_heap()`
- Libc-base: `p $_base("libc")`
- Binary-base: `p $_base()`
- Show Address and surroundings `telescope <addr>`

## gdb config (vanilla gdb)
Just add the following to `~/.gdbinit`:
```
set disassembly intel
set print asm-demangle on
layout asm
layout regs
winheight REGS -2
winheight CMD -5
winheight ASM +5
focus cmd
```

## some nice gdb helpers
The following tools really make your life in gdb easier and enhance the features gdb provides:
- [gef](https://github.com/hugsy/gef)
- [pwndbg](https://github.com/pwndbg/pwndbg)
