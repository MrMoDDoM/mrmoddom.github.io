---
title: ROP Re-call hack
excert: When you hear the call! 
tags:
 - ROP
 - chain
 - call
 - hack
categorie:
 - hack

---

Sometime doing a ROP chain is a real pain, especially when you don't have enough gadets! 

So using a gadgets finder such as [Ropper][1] most often it finds gadgets ending with a ```call```.

This could be very frustrating 'cause a ```call``` will break the rop-chain: just remember that a ```call``` will push a return address on the stack, making your target jumping back from the call right after the gadget.

This situation could be avoided using what a call a ```Re-call hack```.

The concept is to prepare the register that will be used in the call, make it pointing to a simple ```pop``` gadget.

This will cause the address pushed by the call to be poped-out from the stack, leaving your rop-chain intact.

# Example
For example, let's say that you want to use a gadget like this

```python
0x08048700: pop ebx; add eax, ebx; call ecx;
```

This gadget will:
1. ```pop``` the last value on top of the stack in ```ebx```
2. ```Add``` it to ```eax``` and store whitin it
3. Than ```call``` the function pointed by ```ecx```

The last step will also __push__ the address ```0x08048712``` (that point rigth after the gadget) on the stack, causing your program to jump to it once it finds the next ```ret``` instruction in the called function.

That's will break your ROP chain, and we don't like it :smile:

So, let's do this ROP re-call stuff.

It's pretty simple: just find a ```pop ecx; ret;``` gadget somewhere and use it to insert in ```ecx``` the address of another ```pop``` gadget to a register that is free.

Les't say that you find this gadget:

```python
0x0804853a: pop ecx; ret;
```

So now you could make a re-call hack like this:

```python
rop += 0x0804853a # pop ecx; ret; -> This will be called first
rop += 0x0804853a # pop ecx; ret; -> This will pop out the address pushed by the call
rop += 0x08048700 # pop ebx; add eax, ebx; call ecx; -> The nasty one!
```

So, to make things clear, this do:
1. ```pop``` in ```ecx``` the next value found on the stack, 
2. Making ```ecx``` pointing to the same gadget
3. Call the gadget that ends with the ```call```
4. The ```call``` will push it's next address on the stack and call that is pointed by ```ecx```
5. ```ecx``` is pointing to a gadget that will pop in ```ecx``` that nasty address, causing the stack to remaing unaltered

Now your ROP chain can continue without any problems!

That's all! :smile:

---
[1]: https://github.com/sashs/Ropper