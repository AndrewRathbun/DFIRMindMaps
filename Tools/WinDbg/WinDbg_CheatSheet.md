# WinDbg

### Breakpoints

- ​	bp <Addr>: regular breakpoint
- ​	bp <Addr> <Num>: break at the Nth hit
- ​	bu <Addr>: unresolved breakpoint
- ​	bm module!<Regex>: symbols breakpoint
- ​	ba <Access> <Size> <Addr>: memory access breakpoint
- ​	bl: list breakpoints
- ​	bd <Breakpoints>: disable breakpoint
- ​	be  <Breakpoints>: enable breakpoint
- ​	bc  <Breakpoints>: clear breakpoint

### Memory

- Display

  - da: ascii
    du: unicode
    dw: word
    dd: dword
    dq: qword
    db: byte + ascii hexdump
    dc: dword + ascii hexdump
    dW: word + ascii hexdump
    dp: pointer size
    dD: double
    df: float
    dv: local variables
    dt <Type> <Addr>: map struct type to addr

- Edit

  - ea: ascii
    eu: unicode
    ew: word
    ed: dword
    eq: qword
    eb: byte
    ep: pointer size
    eD: double
    ef: float
    eza: null-terminated ascii
    ezu: null-terminated unicode
- Search
    - s -Flags <Range> <Pattern>
        -b: byte
        -w: word
        -d: dword
        -q: qword
        -a: ascii
        -u: unicode
- Fill
      	- f <Range> <Pattern>
- Compare
      	- c <Range> <Addr>
- Move
      	- m <Range> <Addr>

### General

- controls
  - g: go (continue)
  - p: step over
  - t: step into
  - gu: step out

- ?: evaluate expression
  - ? <Num>: hex to decimal
  - ? 0n<Num>: decimal to hex

- lm: list modules

- k: show stack backtrace

- ~: list threads
  - ~<Num>s: switch to thread
  - ~<Num>k: thread backtrace

- |: list processes
  - |<Num>s: switch to process
  - |<Num>k: process backtrace

- r: registers
  - r <Reg>: read register
  - r <Reg>=<Val>: set register

- u: disassemble
  - u <Addr>: disassemble from this address
  - u <Range>: disassemble memory range
  - uf <Addr>: disassemble function

- x: examine symbols
  - x /f module!<Regex>: examine module functions matching this regex

### Bang Commands

- !teb | !teb <Addr>:
  display thread environment block

- !peb | !peb <Addr>: 
  display process environment block

- !handle: list all handles
  - !handle <Val>: get handle type
  - !handle <Val> f: get handle detailed info

- !address: view complete address space

- !address <Addr>: 
  display status of a memory block 
  (region size, protection, ...)

### Meta Commands

- .symfix: set the symbol path to point to the Microsoft symbol store.
- .reload /f module.dll: reload module symbols
- .detach: detach from a process
- .cls: clear commands window
- .childdbg <0|1>: attach to child process

- .writemem <FileName> <Range>: 
  write contents of a memory range to a file