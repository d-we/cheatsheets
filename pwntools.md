# [Pwntools](https://github.com/Gallopsled/pwntools) Cheatsheet

## Getting Started
- Importing: `from pwn import *`
- Set binary context (important for endianess etc.):
  - per binary: `context.binary = "/bin/ls"`
  - hardcoded, e.g., `context.arch="amd64"`
- Start the process:
  - Default: `r = process("/bin/ls")`
  - From gdb: `r = gdb.debug("/bin/ls")`
  - Attach gdb (recommended but buggy lately): `gdb.attach(r)`
- Connect to a remote process: `r = remote("127.0.0.1", 31337)`

## Communication
- Sending bytes:
  - Send bytes: `r.sendline(b"input")` # Sends "input\n"
  - Send bytes: `r.sendline(b"input")` # Sends "input\n"
  - Send bytes (without newline): `r.send(b"input")` # Sends "input"
  - Receive until newline: `r.recvline()`
  - Receive until newline (with timeout): `r.recvline(timeout=1)` # 1 second timeout
  - Receive until pattern: `r.recvuntil(pattern)`
    - e.g., `r.recvuntil("command:")`
    - supports timeout
  - Receive n bytes: `r.recv(8)`
  - Receive till program exits: `r.recvall()`
    - supports timeout

## Byte Patterns
- Convert integers to encoded bytes (and vice versa)
  - `p64(0xdeadbeef)`
  - `p32(0xdeadbeef)`
  - `p16(0xdead)`
  - `p8(0xff)`
- Convert bytes to integers
  - `u64(b"\xef\xbe\xad\xde".ljust(8, b"\x00"))`
  - `u32(b"\xef\xbe\xad\xde")`
  - `u16(b"\xad\xde")`
  - `u8(b"\xff")`

## Working with ELFs
- Load the ELF: `e = ELF("/bin/ls")`
  - supports executables and libraries
- Read GOT entries: `e.got["__libc_start_main"]`
- Search for byte patterns: `e.search("\xc3")`
  - yields a generator, e.g., `next(e.search("\xc3"))` gives the first output
  - supports argument `executable=True` to restrict search for executable regions
- Dump entire section: `e.section(".data")`

## Assembly
First, you need to set your architecture, e.g., `context.arch='amd64'` for x86-64
- Assemble instructions to bytes: `asm("xor eax, eax\nret")`
- Disassemble bytes: `disasm(b"\x31\xc0\xc3")`

## De-Bruijn Pattern
De-Bruijn patterns can be helpful to quickly locate offsets of a access memory access
 1) Create De-Bruijn pattern: `pattern = cyclic(length=100)`
 2) Find an offset: `offset = cyclic_find("aama")`

## Misc
  - Pwntools supports logging:
    - `log.info("msg")`
    - `log.warning("well... anyways")`
    - `log.error("damn.")` # also throws an exception
    - log level can be configured using `context.log_level`
      - e.g., `context.log_level="error"` disables almost all prints


## Further Links

- [Official Documentation](https://docs.pwntools.com/en/latest/)
- [Official Repository (incl. Installation Guide)](https://github.com/Gallopsled/pwntools)
