---
layout: post
title:  "Details on crack tutorial"
date:   2014-09-11 06:54:12
tags:   tutorial crack software learning
---
In this post I'm going to go into more detail, less pictures (maybe) of the process I used to crack the second speech center software. (SSC from now on...)

I started last time with just jumping in and grabbing OllyDbg (one of the greatest debuggers ever written.), grabbing the OllyDump plugin and proceeding to hold your hand and walk you through one of the simplest cracking methods. Now I'm going to go into more detail about why I did what and why it worked, no training wheels. (mostly because this is like riding a tricycle...seriously.)

When we first opened the SSC exe in Olly, we got presented with a big 'ol scary assembly dump of the executable in memory, don't worry too much about it, x86 and other assembly languages can look scary but they're really pretty simple.

Just below the assembly dump is a nice little window that can be super helpful; giving you details about the currently highlighted instruction, such as the current value of processor registers or the action taken on condition jumps (as is the case in the "tutorial")

On the lower left is the hex dump of the executable's memory space (it's also the executable itself), with memory address, hex and ascii output; it's quite neat to see functions being looped to process a string or something, super duper useful at times.

Next to that is the process's stack region, super handy when tracing function calls and stuff. (could come in handy I guess if you're writing a stack overflow exploit *cough*)
Certain things happen when certain instructions are executed by the processor, when a CALL instruction is made, EIP (next code to execute) is PUSHd onto the stack and a JMP is made to the location indicated by the label operand (IE iisc.0040A8EC jumps to the 0040A8EC address ub tge iisc.exe address space), so from that simple example you should hopefully see how useful a visual representation of the stack can come in handy.

The last window is one of the most important (really they're all important, but since this is the last window, it's super duper important), this is the CPU status window (or some such), it contains all the register's current value, processor status flag bits, the flags register, and a bunch of floating point stuff.

Now that I've explained a little about everything in the window, let's continue with the cracking process. To start why did I search for the error message string? Because most of the time it's the simplest check to bypass and if it's successful, you just cracked the software with very little effort. Something I didn't screenshot when I did all this, like 3 years ago, is that in the same section the error message is, lower in memory (higher up in the dump), the success message is there, so I knew I was close to where I wanted to be. Scrolling back up a tiny bit to find the conditional jumps that lead to the error message, setting a breakpoint and rerunning, flipping conditional bit and forcing the success message I was expecting to just get a success message and still have it not register success, but to my surprise it did and it was the quickest cracks ever. (Took me less than 10 minutes to finish)

If I had traced into the iisc.004078c4 instruction right before the JNZ I would have known for certain if skipping the jump would work or not (maybe)

{% highlight nasm %}
CALL iisc.004078c4
JNZ SHORT iisc.005a6493
{% endhighlight %}

For all I know there could have been another check shortly after the error/success call and I would have to step forward to find it by hand, though there's probably ways to find it without doing it by hand, but I'm no professional or master at this, just a tinkerer. And yes, I did put the syntax highlight in just to see if pygments could handle assembly and it can, to a certain extent :D

Continuing on, why did I even add 3 breakpoints? Just because; I knew the last breakpoint was the one I wanted right from the start, I just wanted people to have something to do. xD

I knew this because of the call function right before the jnz and an assumption and my assumption was correct. So why did a I fill the jnz with nops instead of rewriting it to a je? I could have and it would have worked the same, I honestly don't know why I chose nops at this point. Honestly I wrote anddid all of this 3 years ago, so saying "I was thinking exactly this" would be silly, because I was most likely baked off my ass at the time.

The idea is though, bypass the check. If it's checking for a zero and jumping to the wrong place, give it a 1 instead.

Moving on OllyDump now, why did I choose the options I did? Because of the way the binary was made in the first place. It was a dynamic build exe, meaning it loaded dlls int at load time, so I didn't want JMP and Calls in memory to the DLL function stuff; and I don't want the import table to be rebuilt, it's fine the way it is, we didn't mess with it and we don't want it getting fucked. The whole import table I referred to is the Import Address Table and Import Name Table in the PE/COFF spec. (that's the windows executable format) An entire post could be done on that alone, so I won't go into further detail right now, but I suggest anyone reading this to read up on the PE/COFF format and read up on x86 assembly, you don't have to master it, but get to know it.

That pretty much sums it all up, I didn't go into major detail, but I hope you learned a tiny bit of something.
