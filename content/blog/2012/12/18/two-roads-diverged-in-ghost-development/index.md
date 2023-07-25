---
title: "Two roads diverged in Ghost development"
date: "2012-12-18"
tags: 
  - "ghost-d68"
---

Over the last few weeks I've basically rewritten the core of [Ghost](http://code.google.com/p/ghost-usb-honeypot/), our system for USB malware detection. While the new approach promises to be much more effective, it has a drawback: It only works for Windows Vista and later systems. As a consequence, there are now two flavors of Ghost in existence: One supports Windows XP but won't receive much further development, whereas a lot of interesting new features will be implemented for the other one, which is dedicated to Vista and later. In this post, I'm going to explain the reasoning behind the decision, describe the recent technical advances and outline some of our plans for the future.  
  
  
  
The basic concept of Ghost is to detect when a computer tries to write data to removable media without the user's consent. We take the write attempt as an indication of USB malware acting on the system (see my [previous post](https://community.rapid7.com/community/open_source/magnificent7/blog/2012/10/16/ghost--an-introduction) for a more detailed explanation of the idea). This very concept implies that we operate on the same system where our target malware executes - probably with admin privileges. So it is inherently possible for malware to detect Ghost and thwart our efforts. The all-important question is: How difficult is that? Our aim is to make it ever more difficult to infect systems undetected. By the way, I believe that this is true of almost any security solution (like AV scanners, ASLR or DEP) - they are not perfectly secure, but our efforts raise the bar for successful infection. This was the reason for me to reimplement Ghost's core in such a way that it is harder for malware to evade.  
  
Ghost emulates USB flash drives and tricks the malware into believing them to be real (i.e. physical) devices. The system crucially relies on the close resemblance of real flash drives and their emulated counterparts. In previous versions of Ghost, we replaced some of the standard drivers included in Windows with our own to realize the emulation. They would control the emulated flash drives and try to behave just as the standard drivers would for real flash drives. However, this approach required us to duplicate a lot of existing functionality, and large parts of the system relied on educated guesses as to what the operating system was expecting from our drivers.  
  
Starting with Windows Vista, Microsoft introduced a technology called "[virtual Storport miniport drivers](http://msdn.microsoft.com/de-de/library/windows/hardware/ff567541(v=vs.85).aspx)". Originally targeted at manufacturers of high-performance storage controllers, the concept also gives us a means to emulate USB flash drives. The first compelling advantage over the previous approach is that we now communicate with the operating system through a well-defined interface. We no longer have to anticipate what the system might expect as a reply when sending us a certain type of request. The second improvement is that our emulated flash drives are now controlled by the standard disk class driver (the functionality of which we formerly had to duplicate). This means that from the malware's perspective it has become harder to distinguish emulated devices from real ones, because the higher-level functionality of both is provided by the same driver.  
  
While the new concept is a lot cleaner from a programmer's point of view and mitigates evasion of Ghost by the malware, it does have the caveat of not being supported by Windows XP. Let me explain why we still decided to take that step: Ghost is mainly targeted at two types of audience. One is the security research community, where the system might help to analyze malware. I expect that there are people among this audience who use virtual machines running XP for analysis. But for those people Ghost provides the basic features that they need - they wouldn't benefit from the upcoming features, which I'll outline shortly. On the other hand, we have our second audience comprising security-aware companies and organizations that want to protect their systems from USB malware. Many of them will have migrated to one of XP's successors by now, so they will be able to enjoy all of the new features. As you will see, they are the main audience for future improvements anyway. (Which doesn't mean that we don't want to support the research community - rather, the current functionality seems to provide most of what one requires for malware analysis. However, if you are a researcher and have a feature request, please tell me!)  
  
So how do we plan to enhance Ghost in the future? Well, we'll basically try to make use of the network environments found in most companies and employ them to provide a certain level of protection to the computer pool as a whole. Ghost is able to detect the infection of single machines - why not induce actions on other computers upon detection? We will implement ways to run Ghost on all your computers and collect information on possible detections in a single place, where an incident response team is able to review the reports and react immediately. Also, we will build an infrastructure that allows yet uninfected machines to react to an infection of one of their peers - for example, by disallowing USB flash drives for a certain period of time.  
  
As you can see, there are many enhancements to implement, and I'm looking very much forward to the release of the next version. For now, there is a [beta version](http://code.google.com/p/ghost-usb-honeypot/downloads/detail?name=ghost-v0.2.1-beta-win7.zip&can=2&q=) available on the project's website that features the new approach for Windows Vista and later. Since most improvements happened under the hood, you will find the interface mostly unchanged. The install procedure is quite straightforward - just run "setup.exe" and you're good to go.  
  
Let me finish with the usual call to interested people: If you have some time to spare and would like to work for an interesting open source project, please get in touch! No need to be an expert, just be curious and enthusiastic. (Of course, experts are welcome as well!) Also, if you experience any problems running Ghost or would like to share ideas for future development, just contact me! (Contact details are on the project's website.)