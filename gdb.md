# gdb Cheatsheet

## Open gdb
- Start gdb with binary: `gdb <binary>`
- Start gdb with binary and args: `gdb --args <binary> <args>`
- Attach gdb to process: `gdb -p <pid>`

## Handling Execution
- Start execution: `run`
- Start execution and break at start: `start`

- Breakpoint at address: `break <addr>`
- Breakpoint at function: `break <fun-name>`
- Breakpoint at source line: `break <file>:<line-number>`

- Run until next breakpoint: `continue`
- Run until return of functin: `finish`
- Run program until condition is false: `watch <condition>`
- Step to next source-code instruction: `step`
- Step to next assembly instruction: `stepi` or `si`
- 

## Examine Memory
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

## Gather Information
- Show disassembly: `disassemble`
- Show disassembly for function/address: `disassemble <fun-name/addr>`
- Show functions: `info functions`
- Show threads: `info threads`
- Show source-code: `list <filename>:<fun-name/line-number>`

## Miscellaneous
- Handling signals: `handle <signal> <options>`
  * `<options>`:
    * `(no)print`: (Don't) print a message upon catching signal
    * `(no)stop`: (Don't) stop the program upon catching signal
    * `(no)pass`: (Don't) pass the signal to debuggee upon catching signal
  * e.g. `handle SIGSEGV nostop noprint pass` (useful for debugging Java programs`


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