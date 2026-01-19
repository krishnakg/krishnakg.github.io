+++
title = "Getting reliable WiFi on Thinkpad T440s with Linux"
author = ["Krishna Gopalakrishnan"]
date = 2025-04-09T19:33:00-07:00
lastmod = 2025-04-09T19:40:08-07:00
tags = ["linux", "pop-os"]
draft = false
+++

While looking around at my old devices, noticed my trusty old Thinkpad 440s (bought in 2014) which had been gathering dust for a while. Hoping to get some more use out of it, I installed Pop_OS!, but noticed that wifi would get cut-off on a regular basis, and it consistently needed a reboot to get wifi working again. Here is a fun story on how I got it working.

<!--more-->

The Thinkpad T440s from 2014 came in two flavors, one with an Intel wifi card, and the other a Realtek RTL8192EE card. Mine came with the Realtek adapter, and there are enough stories on the internet around its questionable reliability on both Windows and Linux. Further digging showed there is [an alternate driver](https://github.com/lwfinger/rtl8192ee) which had been maintained by Larry Finger, atleast since 2019. Larry Finger [passed away](https://www.iucr.org/news/newsletter/volume-32/number-2/larry-w.-finger-19402024) in 2024, which means we no longer have someone maintaining it. A quick build on the project showed it no longer builds with the latest kernels, as function signatures have changed in the newer kernels. Luckily, it was a fairly trivial fix, and the [resulting code](https://github.com/krishnakg/rtl8192ee) now builds fine for kernel 6.12, which is what Iam currently running. The documentation on how to install it still works well enough. There is an alternate method to install via DKMS, which I did not try.

I also noticed that the card does not wake up after it goes to sleep, so I have disabled wifi.powersave at `/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf` with the value `wifi.powersave = 2`. (2 disables powersave, 3 enables it.) Some more information on the value [here](https://gist.github.com/jcberthon/ea8cfe278998968ba7c5a95344bc8b55). In addition, I have also disabled Intel Rapid Start amd enabled CPU power management in the BIOS.

Overall, this seems to have fixed the regular wifi disconnections.
