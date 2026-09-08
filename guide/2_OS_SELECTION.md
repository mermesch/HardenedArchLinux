## 2_OS_SELECTION

## Windows

Windows is not secure: it records your voice, makes and uploads screenshots, keylogs everything you type, and has backdoors. https://www.kicksecure.com/wiki/Windows_Hosts

## macOS

Has intentional backdoors that allow remote root privileges and poses many other security and privacy threats. https://www.whonix.org/wiki/Host_Operating_System_Selection#macOS_Hosts

## Linux

Don't assume your new Linux system is safe right out of the box. You have to lock it down yourself.

### Frozen packages: the update problem

The problem comes from how popular systems like Ubuntu and Debian handle updates. They freeze apps at one version for years. When a bug is found, they only fix it if it gets an official security label (called a CVE).

But here is the catch: most fixes never get that label. Sometimes the person who wrote the code doesn't ask for one, or they don't even realize the bug can be used by hackers. Because of this, the people who maintain these Linux systems miss a ton of security fixes. They simply don't have the time to read every single change made to the software.

The core of the operating system—the kernel—is especially bad at handing out these labels. The results are pretty scary. For example, Debian's version of a popular web browser is still full of known holes that hackers are actively using right now. Even worse, if there is no public example showing exactly how a hacker would attack a major system flaw, Debian might leave it broken for over a month before fixing it.

### Rolling releases

There is a better way: "rolling" Linux systems. These versions update constantly. The second a fix is created by the developer, you get it. You don't have to wait around or rely on a broken labeling system to stay safe.

### Choosing a rolling system: Arch, Gentoo, or Fedora

So, which rolling system should you pick? Arch fixes security holes the fastest. Fedora and Gentoo are also solid choices. Honestly, Gentoo is technically the safest option because it lets you avoid systemd and use a different core system called musl. However, I am going to write this guide for Arch instead. This isn't because I think Arch is better than Gentoo. I don't have the time to write a Gentoo guide.

Arch gives you the quickest fixes and is less work to set up. In our setup we only use the main computer for updating. We do all our actual work inside a Virtual Machine (VM). If a hacker is good enough to break out of that VM, they could probably take over a Gentoo system just as easily. If you use a firewall to block your local network, there is almost no real-world safety difference between Arch and Gentoo for offline or local attacks.

If you want total perfection, go with Gentoo. However, if you do, you must be very careful. You will need to configure the boot and shutdown sequence of your services correctly. For example, if your firewall and VPN are shut down at the same time, your real IP address could be leaked. This is just one example, there are many other risks if something is misconfigured.

Finally, if setting up Arch sounds like too much work, but you still want something safer than Debian or Ubuntu, choose Fedora. It gives you much faster security updates than those frozen systems.


## References:
- https://www.kicksecure.com/wiki/Essential_Host_Security#Host_Security_Essentials
- https://www.whonix.org/wiki/Host_Operating_System_Selection
- https://www.kicksecure.com/wiki/Windows_Hosts