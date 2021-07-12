# MICROCORRUPTION

 
## HANOI WRITEUP


Author: [Udit Prasad](https://github.com/uditpd3000)

### Walkthrough
When I went through the main function, it was only calling Login function so I tried to explore the login function whereI found out this:

```
4520 <login>
4520:  c243 1024      mov.b	#0x0, &0x2410
4524:  3f40 7e44      mov	#0x447e "Enter the password to continue.", r15
4528:  b012 de45      call	#0x45de <puts>
452c:  3f40 9e44      mov	#0x449e "Remember: passwords are between 8 and 16 characters.", r15
4530:  b012 de45      call	#0x45de <puts>
4534:  3e40 1c00      mov	#0x1c, r14
4538:  3f40 0024      mov	#0x2400, r15
453c:  b012 ce45      call	#0x45ce <getsn>
4540:  3f40 0024      mov	#0x2400, r15
4544:  b012 5444      call	#0x4454 <test_password_valid>
4548:  0f93           tst	r15
454a:  0324           jz	$+0x8
454c:  f240 0b00 1024 mov.b	#0xb, &0x2410
4552:  3f40 d344      mov	#0x44d3 "Testing if password is valid.", r15
4556:  b012 de45      call	#0x45de <puts>
455a:  f290 9b00 1024 cmp.b	#0x9b, &0x2410
4560:  0720           jne	#0x4570 <login+0x50>
4562:  3f40 f144      mov	#0x44f1 "Access granted.", r15
4566:  b012 de45      call	#0x45de <puts>
456a:  b012 4844      call	#0x4448 <unlock_door>
456e:  3041           ret
4570:  3f40 0145      mov	#0x4501 "That password is not correct.", r15
4574:  b012 de45      call	#0x45de <puts>
4578:  3041           ret
```
Here i noticed `test_password_valid` function but it was of no significant use and then
 `455a:  f290 9b00 1024 cmp.b     #0x9b, &0x2410` caught my eyes which was directly leading to implementation of `unlock_door`.
And also i noticed that the password is getting stored from #0x2400.
So it is comparing 17th character of our password with `#0x9b`.
So we can give any random 16 characters followed by 9b as payload which will do the trick.

So i tried with `9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b` as hex input and yes it worked as expected.     

### Password

```9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b9b```
