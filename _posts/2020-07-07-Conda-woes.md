---
layout: post
title: Conda Woes
---

As is implied by my earlier post, I'm one of those students that ignored suggestions on buying a Mac for this bootcamp. I did the prework just fine on a windows machine, and I can always install linux on my laptop, right?

Well, Microsoft decided to throw me a bone and released WSL2 to the public, which basically is a great Linux Virtual Machine integrated into Windows, right before the Bootcamp Started.

I decided to try to install all the Metis things on both my Windows and in my Linux installation, and record what happens for future students to learn from me.

First thing was setting up Conda and the other enviroments in both Windows and Linux.

Well, there's your first problem.

It seems that the XGBoost algorithm's Windows version was depreciated and downgraded, and causes the Metis enviroment to fail to fully install on Windows no matter what. Good thing I'm installing it on Linux as well!

The installation seems like it would be fully functioning once the windows XGBoost module gets put back on to Conda Forge, and even if it doesn't, once the alternate installtions are avaialble \(which they weren't at this point in time, with links leading to dead downloads\) everything should work.

I'm sure that's not important at all... oh wait, we're going to go over it this week. :\(

On the other hand, it installed with only a few out of date modules on the WSL2 Linux VM. There was some downgrading that needed to happen and a few prompts that you can't miss or else the installer will cancel itself or crash out, but it's relatively painless on Linux. So that's fine at least.

Once Conda is installed, we have to set up Git. I decided to use one of my windows folders as my git folder, which can get very annoying to constantly navigate back to from the linux command line.

Since WSL2 is a virtual machine, I have to navigate out of the Linux file system and into the Windows one.

There's a shortcut to do it, but it's easier on myself to modify my Windows Terminal Initialization File so that when I launch my Linux Ubuntu command line in one of the tabs, it defaults to my windows C drive.

This is actually a great idea, as if I need to get back to the home directory of my Linux install, the classic "cd" does that instantly.

What's fustrating is that while Windows Terminal allows you to add more launchable apps into the configuration file, even with a separae unique ID, I can't seem to get it to have the same operating system command line in the menu twice, even with different launch folders. So I can either have a Ubuntu command line that launches in the Linux home directory by default in the quick access menu, or one that launches from my Metis folder on my C drive, but not both.

Another thing that is pretty annoying is that Windows and Linux uses different slashes for the address bar. Windows uses \\ while Linux uses /. This confuses me a lot, especially with the shortcuts

```
To access Linux file system from windows:

\\wsl$

To access C drive from Linux:

/mnt/c/
```

With that out of the way, Conda was set up successfully, and there has been no problems ... yet.

Well, except the fact that copy paste is different in Windows and Linux, even when copy pasting across tabs in Windows Terminal.

But I'll figure that out with how to get Linux GUI apps to work as well down the road.


P.S.

For anyone on Linux, WSL or not: IGNORE LINUXBREW. It does nothing. It helps nothing. It will actively make your life miserable to install, and it is useless because the one function it does that maks you want it on your mac... doesn't work on Linux.

