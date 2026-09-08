## 08_PRE_INSTALLATION_INFORMATION

**NOTE:** This guide asks you to reboot many times. This is deliberate. Reloading systemd can cause VPN leaks with many VPNs, so after unit changes we reboot

## MAC (Mandatory Access Control)

Classic Unix permissions are discretionary. Whoever owns a file decides who may read, write, or execute it, and every program you run inherits all of your rights. A compromised browser can walk away with your SSH keys, your documents, and every secret in your home directory.

Mandatory Access Control closes that gap. The kernel enforces an administrator-defined policy on every process, and no program can step outside the rules assigned to it, no matter which user it runs as. Exploiting a service no longer hands the attacker everything that service could touch.

Linux gives you two serious options.

SELinux attaches security labels to every process and file and polices every interaction between them. It is the strongest mainstream MAC: fine-grained, battle-tested, and the default on Fedora and RHEL. Its cost is complexity. Policies are enormous, denial messages are cryptic, one bad setting can break the boot, and on Arch the support is minimal and mostly manual. Running SELinux on Arch is a project of its own.

AppArmor confines individual programs with profiles that spell out which paths and capabilities they may touch. Profiles are plain text, path-based, quick to audit, and the apparmor.d project maintains a large collection of them. Ubuntu and openSUSE ship it as their default, and it integrates cleanly with Arch.

The honest tradeoff: SELinux is stronger. It confines more, deeper. But protection you cannot configure correctly protects nothing. On a rolling Arch system, AppArmor gives us enforceable profiles today, while SELinux would give us a maintenance burden that outlives our patience.

Decision: SELinux is out of scope. This guide uses AppArmor.

Know the limit. AppArmor only restrains programs that have a profile, everything else runs unconfined. So we profile what faces the network


## Systemd hardening

Every background service on this system runs as root by default: DHCP clients, DNS, the display manager, the time daemon. Each one is a program with bugs, running with unlimited rights, waiting on network input. Compromise any of them and the whole machine falls.

systemd can cage each service in its own sandbox. One drop-in file strips a unit of almost everything it does not need.

Most options fall into four families.

1. File system isolation: ProtectSystem=strict makes the whole file system read-only except what you explicitly hand back with ReadWritePaths=, ProtectHome= seals off user data, PrivateTmp= gives the service its own /tmp that other processes cannot see, and InaccessiblePaths= hides everything else it has no business touching.
2. Privilege: User= drops root entirely, CapabilityBoundingSet= removes individual capabilities like CAP_SYS_ADMIN, NoNewPrivileges= kills setuid escalation, and UMask=077 keeps whatever it writes private.
3. Kernel surface: ProtectKernelTunables=, ProtectKernelModules=, and ProtectKernelLogs= lock the parts of the kernel the unit should never write to, ProtectProc=invisible hides other processes, RestrictAddressFamilies= cuts network families like AF_PACKET it does not use, and SystemCallFilter= blocks entire syscall classes.
4. Escape prevention: MemoryDenyWriteExecute= blocks self-modifying code, LockPersonality= blocks ABI-switch tricks, RestrictNamespaces= blocks namespace escapes, and DevicePolicy=closed with DeviceAllow= whitelists device nodes.

These are defense in depth. The service still gets compromised through its own bugs, but the attacker lands in an empty cell with no files, no caps, no syscalls, and no neighbors.

The cost is breakage: the wrong restriction kills the service. Our method is always the same: copy the unit, apply the strictest sandbox that still works, then loosen one line at a time until it runs.

Our own method of testing is built in: run systemd-analyze security on the unit. It scores exposure from 0.0 (locked down) to 10 (unsafe). The default NetworkManager unit sits near 9.6. The drop-in in file 5 brings it under 2.0.

One warning: sandboxing is not a replacement for the AppArmor MAC layer and systemd restrictions overlap, they do not substitute. We stack both.


## Cryptography and future-proofing

The NSA, CISA, and NIST are saying the same thing: nation-state attackers are recording encrypted traffic today and storing it for later. The attack even has a name, "harvest now, decrypt later". It works because most of today's public-key cryptography, RSA and elliptic curves, falls to a large quantum computer running Shor's algorithm.

Symmetric crypto survives: quantum search only halves key strength, so AES-256 still stands. The exposed part is the key exchange and the signatures behind TLS, SSH, and messaging.

Anything whose secrecy must outlive the next decade is already at risk, because the capture happens now, not in the future.

NIST has published its first post-quantum standards (ML-KEM, ML-DSA, SLH-DSA) and the official advice from every agency is identical: inventory your cryptography and migrate to the new standards as soon as practical.

This guide already leans that way. The disk is encrypted with LUKS2 and Argon2id, and the VPN section picks Mullvad partly because it tunnels with a hybrid of ML-KEM and Classic McEliece: a break in one algorithm does not break the connection.

Post-quantum is a moving target. Follow the NIST announcements, and whenever software offers a hybrid mode, take it.


## Package management and system minimalism

The most secure package is the one you never installed. Every package is code that runs with your privileges, parses input from somewhere, and needs updating forever. A minimal system is not an aesthetic choice, it is the cheapest security win available.

Arch's official repositories are built and signed by trusted developers, and pacman verifies every signature before anything touches your disk. That is the default trust level of this entire guide.

The AUR offers no such guarantee. Anyone can publish a PKGBUILD, and what it downloads is never signed by Arch. There have been many malicious packages in the AUR lately. Treat every AUR package as untrusted code: read the PKGBUILD, check where it fetches from, and only then build. The Mullvad installation in the VPN section shows the full careful procedure.

Minimalism continues after installation: remove packages you stopped using, keep the service mask list from the app installation section current, and update regularly. On a rolling release, security fixes arrive as ordinary updates, and an Arch box that skips them collects vulnerabilities while looking perfectly healthy.


## References:
- https://www.kicksecure.com/wiki/Advanced_Documentation
- https://privsec.dev/posts/linux/desktop-linux-hardening/
