# Radare2 Cheatsheet

## Open r2
- open binary standard way: `r2 <binary>`
- open binary in debug mode: `r2 -d <binary>`
- attach to process: `r2 -d <pid>`
- open in write mode: `r2 -w <binary>`
- open file without parsing header - useful for hexfiles: `r2 -n <file>`

## Navigate
- seek to address: `s <addr>` -- works for flags aswell
- temporary(for the given command) seek to address: `<cmd> @ <addr>`
- open visual mode: `v` -- you can switch to graph mode using space-key
- open visual panel mode: `V!`

## Visual Mode
- enter interactive flag search: `_`
- choose function interactive: `v`
- show x-refs to current function: `x`
- show x-refs from current function: `X`
- seek to instruction pointer - use in debug mode: `.`
- undo/redo seek: `u` and `U`
- switch visual views: `p` and `P`
- stepping - use in debug mode: `s`
- add/remove breakpoint at current seek: `<F2>`
- step / step over / continue: `<F7>` / `<F8>` / `<F9>`
- enter/leave cursor mode: `c` - use tab to switch cursor between views

## Gather Information
- infos(pic, relro, ...) about binary: `iI`
- show shared libraries: `il`
- show imports: `ii`
- show strings in .data: `iz`
- show strings in the whole binary: `izz`
- show sections: `iS` -- for dynamic loaded sections like heap & stack use `dm`
- show sections at current seek: `iS.`
- show memory maps - use in debug mode: `dm` -- shows e.g. libc-base
- show current memory maps - use in debug mode: `dm.`
- show loaded modules(libraries, ...) - use in debug mode: `dmm`
- retdec-decompiler: `pdd?`
- search commands: `/?`

 ## Analysis
- analyse most things: `aaa`  -- prefer other commands for large binaries or in debug mode
- analyse functions at symbols: `aa`
- analyse function calls - useful for finding functions fast:  `aac`
- autoname functions: `aan`
- search for C++ vtables: `av`
- parse RTTI for each vtable: `afra`
- show all functions and the functions they call: `aflm`

## Annotate
- change current function name: `afn <new_name>`
- change local variable name: `afvn <new_name> <old_name>
- display local variables: `afvd`
- change variable type: `afvt <name> <type>`
- manipulate register arguments: `afvr?`
- type commands: `t?` - TODO

## Debug
- analyse syscall - use in debug mode: `as`
- continue until address: `dcu <addr/flag>`
- set breakpoint: `db <addr/flag>` 
- step to next instruction: `ds`
- show register contents: `dr`
- set register value: `dr <reg>=<val>`
- analyse heap: `dmh?`

## Various Commands 
- print string: `ps`
- print hexdump: `px`
- print disassembly: `pd`
- print hex number: `?v <expression/flag>`
- print decimal number: `?vi <expression/flag>` 
- more printmodes: `???`


- show assembly instruction description: `aod`
- translate syscall number to syscall name: `asl <syscall_number>`
- translate syscall name to syscall number: `asl <syscall_name>`

- run tasks in background: `& <cmd>`
- show background tasks: `&`
- join task: `&& <task_number>

- show help of cmd: `<cmd>?`
- open cmd output in r2-internal less: `<cmd>~..`
- open cmd output in r2 interactive search: `<cmd>~...`
- r2 internal grep: `<cmd>~<keyword>`
- export informations to r2-script: `<cmd>* > run_me.r2` - all commands ending in * will output r2 commands
- TODO: config variables


## Pwning
- show offset in shared library: `dmi <so_name> <symbol>` - e.g. `dmi libc system`
- search rop gadgets: `/R <instruction>` - e.g. `/R pop rdi` - use 'e search.in' to control where gadgets are searched
- search rop gadgets using regex: `"/R/ <regex>"`
- visual rop builder: `vbr` - newly implemented, hence lacks features
- generate De Bruijn pattern: `ragg2 -P <length> -r`
- find offset in De Bruijn pattern: `wopO <value>` - e.g. `wopO 0x41424344`

## Debugging forking binaries:
- commands related to child processes: `dp?`
- follow child while debugging: `e dbg.follow.child = true` - last time I checked this it was broken

### Workaround for debugging childs:
- 1: use `e dbg.forks = true` - will set breakpoint on child aswell
- 2: set breakpoint after the fork & continue
- 3: on breakpoint(after fork) use `dpc`, which will switch to the latest forked child

## Debugging using 2 terminals:
1: On Terminal-1: `tty` output will be referred as $tty. 
   On Terminal-1: `sleep 99999`

2: Create rarun-profile with the following content:
   ```
   #! /usr/bin/rarun2
   stdio=$tty
   ```
3: On Terminal-2: `r2 -r <profile_file>.rr2 -d <binary>`

## Writing ELF-header:
- TODO

## Additional resources:
- https://radare.gitbooks.io/radare2book/
- http://radare.today/
- https://github.com/dukebarman/awesome-radare2
