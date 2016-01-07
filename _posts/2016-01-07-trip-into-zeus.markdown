---
layout: post
title: "A Trip Into ZeuS"
date: 2016-01-07 09:00:00
tags: malware sourcecode documentation
---
Any kind of in-depth look into the ZeuS source code just isn't there....


At least not as far as I can find. I'm not writing these posts of "documentation"
on the ZeuS source code for the public really, but might as well publish what I
document. All of my documenting is more of a personal learning experience. Some
of it may be wrong, buteventually it will be corrected. I hope.


Might as well start at the core.

"/source/client/core.cpp"

Down at the very bottom is _entryPoint(), I'm assuming that it's the
 entry point of the bot. (But what do I know?)
{% highlight cpp %}
void WINAPI Core::_entryPoint(void)
{
  bool ok = false;

  if(init(INITF_NORMAL_START))
  {
{% endhighlight %}
INITF_* are enum constants; INITF_NORMAL_START has a value of 0x0.<br />
Core::init() starts with an if check with a bitwise & inside it.
{% highlight cpp  %}
if((flags & INITF_INJECT_START) == 0)coreData.proccessFlags = 0;
{% endhighlight %}
Since *_NORMAL_START is 0, the & operation returns 0, causing the if statement to set coreData.processFlags to 0.<br /> Core::init() continues by
initializing various classes, debug checks, etc. Core::_entryPoint continues, if
 init() succeeds, to set a few run variables and creates a pointer to arguments.
<br />
CWA() is a #define in /source/client/defines.h
{% highlight cpp %}
int argsCount;
LPWSTR *args = CWA(shell32, CommandLineToArgvW)(CWA(kernel32, GetCommandLineW)(), &argsCount);
{% endhighlight %}
CWA() is an odd way of calling win32 dll functions.<br />
In the line above, we're calling CommandLineToArgvW from shell32 and passing the return value (I think) of GetCommandLineW from kernel32 and the memory address of argsCount.<br />
--Will put more info in when more details are known about the two functions... --<br />
Seeing how all the CWA() calls are to windows dlls, I'm guessing  CWA stands for something like Call Windows Api, or similar.<br />
After *args is filled, it's checked for NULL; if it's not null, we loop through and start setting variables to true or false based on args.<br />
Skipping over the if..elseif...else block, assuming that isInfo and isVnc were not set, runAsBot() is ran.

runAsBot() is where things start to get interesting. :)<br />
A handful of variables are created and a character array, 50 chars wide.<br />
First thing that runAsBot() appears to do is load itself into RAM and decrypt itself.<br />
The RC4 encryption code used is down right sexy. Whoever wrote this code knows what they're doing. They're certainly not native to the English language, but they're no slouch to C/C++.
