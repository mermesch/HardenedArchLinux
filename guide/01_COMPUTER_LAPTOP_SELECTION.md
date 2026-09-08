## 01_Computer Selection, Hardware Threat Minimization and habits

Trusted computer hardware is fundamental to security. It is recommended to purchase and use "clean" computers that have components manufactured by reputable companies.

If you want to hide your identity, it is preferable to pay in cash so hardware IDs do not leak your identity.

It is safest to purchase a computer that is solely used for activities under the hidden identity, because this minimizes the risk of a prior hardware compromise or identity exposure.

## Buying a laptop

If you want to buy a laptop, buy one with a LAN port, not an ultra-thin screen. The microphone and webcam are often located at the top of your screen and are very hard to remove on laptops with ultra-thin screens.

Only DELL, HP, Lenovo and System76 have good firmware update support for Linux. I think Lenovo's good support is mostly limited to the ThinkPad series. These vendors publish their firmware updates to the LVFS (Linux Vendor Firmware Service), so fwupd can install BIOS and other firmware updates directly from Linux. Check the vendors https://fwupd.org/lvfs/vendors/. With most other vendors, you need Windows or a bootable USB stick from the vendor to update your firmware, which means vulnerabilities stay unpatched. Consider buying one of those four brands.

Buy a laptop that does not brick when you remove or replace the Microsoft Secure Boot keys.

## Removable components

### Speaker

A speaker can be used as a microphone to record you, and ultrasonic speaker-to-speaker communication by a nearby device can be used to transmit data between computers.
https://www.kicksecure.com/wiki/Hardware_Threat_Minimization#Profiling_Threat

### Microphone

Same attack surface as the speaker.

### Beeper

Same attack surface as the speaker and microphone.

### Webcam

Can record you and your screen. Anything disabled at the software level can be enabled again.

### Battery

Gives you a way to quickly power off your laptop in an emergency (you should always use the power off option of your OS if there is no emergency). Electricity to your motherboard can keep secrets in your RAM alive for a longer time, which makes you more vulnerable to cold boot attacks. Unplugging your electricity cable after shutdown is a recommended measure. This measure does not work when you have a battery in your laptop.

### Wi-Fi and Bluetooth connector

Wi-Fi and Bluetooth are insecure protocols and can also leak critical identificators like the SSID of your neighbours Wi-Fi to Geoclue servers. Use a LAN cable.

### External input devices

Keyboards and mice: use cables. Avoid wireless keyboards and mice because most send data unencrypted. Even if this was not the case, the robustness of the cryptography involved in proprietary products cannot be verified. A local adversary up to 100 meters away can sniff keystrokes and inject their own, allowing them to take over the machine.

Headphones: avoid headphones. They have the same attack surface as internal speakers and microphones. Wireless ones are more dangerous.

## Habits

### Devices with a speaker or microphone

Do not have devices with speakers or a microphone in the same room as your computer. Any device in your room with a speaker or microphone can record your keystrokes. AI can convert the audio of your keystrokes to text and reveal your passwords and activity.

### Phone

Do not keep a phone with a camera in your room. Phones have weaker security than your hardened computer. A compromised phone can record while you don't know. You can make the mistake of pointing the camera at your screen. The microphone and speakers can listen to your keystrokes. You can remove the camera, speaker and microphone from your phone.

### Network

Preferably connect to an isolated network, not to ADSL or a router with other devices on it. Otherwise your computer is accessible to other nodes on the network, like printers, phones, computers and laptops. Documentation is out of scope for this manual.

### Router

If you use a router, consider buying a machine that is only used to update your router firmware. This reduces the attack surface of the computer you use for work. If you can't do that, use SSH (local SSH, not remote SSH) instead of a browser to update your router. Browsers can be exploited more easily.

### LAN cable

Put in your LAN cable after boot and take it out before shutdown. If your VPN is misconfigured, or fails in a rare edge case that you or the VPN software missed, your traffic can go out with your real IP. With no cable connected during boot and shutdown, nothing can leak.

This is one more reason to remove the Wi-Fi card. Since a Wi-Fi card would have the same issue here, it could auto-connect to a network before the VPN is up or down.

### Leaving your laptop

Never leave your laptop on and running without you being there. While it runs, your disk is unlocked and the keys sit in your RAM. Anyone with brief physical access can read them out or install a backdoor. Locking the screen is not enough, suspending is not enough (RAM stays powered). Only powered off is safe.

### BIOS

Update your BIOS to patch vulnerabilities. Vendors fix real flaws this way, like the BlackLotus bootkit that bypassed Secure Boot. If your laptop is supported, use fwupd (`fwupdmgr refresh` and `fwupdmgr update`), otherwise use the update from your vendor's official website. Good fwupd support is mostly limited to DELL, HP, Lenovo and System76 machines, which is another reason to buy one of those (see "Buying a laptop"). Keep the laptop connected to power, and re-check your BIOS settings afterwards, because an update can reset them and clear your Secure Boot keys.

### Windows and walls

I have no recommendations here, but I want to make you aware. Keystrokes can be recorded through your wall by an attacker on the other side of the wall. Not by sound, but by waves on another spectrum. Keystrokes can be recorded by an infrared laser pointed at your window. If you sit close to a window, this attack is easier to execute.
https://www.thehackacademy.com/news/cutting-edge-spy-tech-can-listen-to-your-conversations-through-windows/
The recorded keystrokes can be converted to text by AI.

## Advanced (out-of-scope) computer selection

Buy a computer that supports disabling Intel ME and installing a libre BIOS such as Libreboot. The NSA has a backdoor in Intel and AMD processors. They can access your computer. It is very hard to disable this.
https://libreboot.org/faq.html#intelme
https://libreboot.org/faq.html#amd

Some companies sell custom computers with Intel ME disabled. However, only high-risk users typically buy these machines. This raises a serious question: can the sellers be trusted?

Buying one might actually make you less safe. It creates a perfect honeypot, allowing attackers to easily pinpoint high-value targets instead of having to search through millions of normal computers.

## Read more about physical security

This file only covers the basics. If you want to go deeper into physical security, read these:

- https://madaidans-insecurities.github.io/guides/linux-hardening.html#physical-security
- https://www.kicksecure.com/wiki/AEM
- https://www.kicksecure.com/wiki/System_Hardening_Checklist#Disabling_and_Minimizing_Hardware_Risks
- https://www.kicksecure.com/wiki/Hardware_Threat_Minimization

## References:
- https://www.kicksecure.com/wiki/System_Hardening_Checklist#Disabling_and_Minimizing_Hardware_Risks
- https://www.kicksecure.com/wiki/Hardware_Threat_Minimization
- https://madaidans-insecurities.github.io/guides/linux-hardening.html#physical-security
- https://www.kicksecure.com/wiki/AEM
- https://libreboot.org/faq.html
- https://www.thehackacademy.com/news/cutting-edge-spy-tech-can-listen-to-your-conversations-through-windows/
- https://fwupd.org/lvfs/vendors/
