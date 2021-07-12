# MICROCORRUPTION

## Johannesburg Writeup

Author:[Udit Prasasd](https://github.com/uditpd3000)

### Walkthrough
After going through login function it appeared to be quite similar with previous level `cusco`. But here we find something interesting
```
4578:  f190 1400 1100 cmp.b	#0x14, 0x11(sp)
457e:  0624           jeq	#0x458c <login+0x60>
4580:  3f40 ff44      mov	#0x44ff "Invalid Password Length: password too long.", r15
4584:  b012 f845      call	#0x45f8 <puts>
4588:  3040 3c44      br	#0x443c <__stop_progExec__>
458c:  3150 1200      add	#0x12, sp
4590:  3041           ret
```
Here we can see that the 17th offset is compared with #0x14 so we need to find can we overwrite that memory.
After debugging with a random password i found out that the stack pointer at #0x4578 is pointing at #0x43ec and at the same place our password is stored.
So we can overwrite it by taking 18th char in our input as `#0x14`.
But even after that we cannot jump to `unlock_door` function but we will always jump it. 
so we will now use the technique which we used in previous level i.e. `Cusco` by returning the address of desired function as return address of login.
So i saw that the stack pointer at time of return call was pointing at `#0x43fe` which is exactly after the 18th character. 
so i put the address of `unlock_door` keeping in mind the little endian nature. 
so i tried the password `4141414141414141414141414141414141144644` as hex input and yes it worked.

### Password
```
4141414141414141414141414141414141144644
```
 
