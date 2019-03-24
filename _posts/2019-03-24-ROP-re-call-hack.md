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

# ROP Re-call hack

Sometime doing a ROP chain is a real pain, especially when you don't have enougth gadets! 
So using a gadgets finder such as [Ropper][1] mosft often it finds gadgets with a ```call``` register in it.
This could be very frustrating 'cause a ```call``` will break the rop-chain: just remember that a ```call``` will push a return address on the stack, making your target jumping back from the call rigth after the gadget.

This situation could be avoided using what a call a ```Re-call hack```.

The concept is to prepare the register that will be used in the call, make it pointing to a simple ```pop``` gadget.

This will cause the address pushed by the call to be poped-out from the stack, leaving your rop-chain intact.

# Example
Let's see an example 

[TBD]


---
[1]: https://github.com/sashs/Ropper