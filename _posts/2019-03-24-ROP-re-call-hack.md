---
title: ROP Re-call hack
excert: When you hear the call!
tags:
 - ROP
 - chain
 - call
 - hack
category:
 - hack
---

Sometime doing a ROP chain is a real pain, especially when you don't have enough gadgets! 

You could use a gadgets finder such as [Ropper][1] and most often you'll find gadgets ending with a ```call```: this could be very frustrating 'cause a ```call``` will break the rop-chain. Just remember that a ```call``` will __push a return address__ on the stack, making your target jumping back from the call right after the gadget.

This situation could be avoided using what I call a ```Re-call hack```.

The concept is simple: prepare the register that will be used in the call, making it pointing to a simple ```pop``` gadget: this will cause the address pushed by the call to be poped-out from the stack once the call is reach, leaving your rop-chain intact.

# Example
For example, let's say that you want to use a gadget like this

```python
0x08048700: pop ebx; add eax, ebx; call ecx;
```

This gadget will:
1. ```pop``` the last value on the top of the stack in ```ebx```
2. ```Add``` it to ```eax``` and store the result in ```eax```
3. Than ```call``` the function pointed by ```ecx```

The last step will also __push__ the address ```0x08048712``` (that point right after the gadget) on the stack, causing your program to jump to it once it finds the next ```ret``` instruction in the called function.

That will break your ROP chain, and we don't like it :smile:

So, let's do this ```ROP Re-call``` stuff.

Les't say that you find this gadget:

```python
0x0804853a: pop ecx; ret;
```

So now you could make a ```Re-call``` hack like this:

```python
rop += 0x0804853a # pop ecx; ret; -> This will be called first
rop += 0x0804853a # pop ecx; ret; -> This will pop out the address pushed by the call
rop += 0x08048700 # pop ebx; add eax, ebx; call ecx; -> The nasty one!
```

So, to make things clear, this do:
1. ```pop``` in ```ecx``` the next value found on the stack, ...
2. ...making ```ecx``` pointing to the same gadget
3. Call the gadget that ends with the ```call```
4. The ```call``` will push its next address on the stack and call what is pointed by ```ecx```
5. ```ecx``` is pointing to a gadget that will pop in ```ecx``` that nasty address, causing the stack to remain unaltered

That's all! Now your ROP chain can continue without any problems! :wink:

---
[1]: https://github.com/sashs/Ropper