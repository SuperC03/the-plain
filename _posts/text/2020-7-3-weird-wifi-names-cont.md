---
title: Continuation of Comparing how different devices display the SSID "á̶̛̛̓̿̈͐͆̐̇̒̑̈́͘͝aaa"
date: 2020-07-03
---

This is a continuation of my previous post [Comparing how different devices display the SSID "á̶̛̛̓̿̈͐͆̐̇̒̑̈́͘͝aaa"](https://hamptonmoore.com/posts/weird-wifi-name-display/). After posting it on Hackernews I got lots of feedback. The key one was something that I had sadly missed when I originally started this project. When the wifi name of "á̶̛̛̓̿̈͐͆̐̇̒̑̈́͘͝aaa" got shortened down to a character before the second a. The issue was this character is actually two characters wide, so the first part of the character stayed, with the second byte of it missing. The character in question was ◌̈́, whos hex codepoint is cd84. Looking at the raw hex of the SSID you can see it ends in cd, `61ccb6cc81cc93ccbfcc88cc9bcc9bcd90cd98cd86cc90cd9dcc87cc92cc91cd61ccb6cc81cc93ccbfcc88cc9bcc9bcd90cd98cd86cc90cd9dcc87cc92cc91cd`. For those curious about how that would be rendered on your device here it is "á̶̛̛̓̿̈͐͆̐̇̒̑͘͝�". After realizing this I decided my previous comparisons of how devices reacted to my weird unicode SSID was unfair. The SSID this time is "á̶̛̛̓̿̈͐͆̐̇̒̑͘͝a" which is almost the exact same but that last two byte long character was replaced with an "a". This was to keep the SSID at 32 octets long.

Below are the previous photos of the previous SSID "á̶̛̛̓̿̈͐͆̐̇̒̑͘͝�" with the new SSID "á̶̛̛̓̿̈͐͆̐̇̒̑͘͝a" under it. They are also grouped the same as previously to keep continuity.

Galaxy S8 running Android 9 with Kernel 4.4.153
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/android.jpg)

![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/android.jpg)

Amazon Firestick
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/firestick.jpg)

![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/firestick.jpg)

As before the Galaxy S8 and the Firestick both handled it perfectly with there being no issues with how it rendered besides vertical cutoff which makes sense.

iPhone running iOS 13.5.1
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/iphone-ios1351.jpg)
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/iphone-ios1351.jpg)

Apple TV Second Generation
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/appletvgen2.jpg)
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/appletvgen2.jpg)

These results were interesting. It showed that if the UTF-8 character in an SSID is invalid then IOS falls back to the [Mac OS Roman](https://en.wikipedia.org/wiki/Mac_OS_Roman) character set, but since the new SSID is valid UTF-8 iOS properly renderers the SSID.

2012 Macbook running High Sierra 10.13.6
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/macos.jpg)

Unlike last time the Macbook actually showed the network. I belive last time instead of falling back to Mac OS Roman like iOS did the Macbook just treated the SSID as spam or noise and dropped it

Windows 10 Pro 10.0.19041
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/windows10.png)

![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/windows10.jpg)

Windows was able to properly handle this SSID as opposed to falling back to [Windows-1252](https://en.wikipedia.org/wiki/Windows-1252) like it did previously.

Chromebook running ChromeOS 83.0.4103.97
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/chromeos.jpg)

![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/chromeos.jpg)

Unlike last time the Chromebook was able to render this perfectly with no question marks.

Kindle Paperwhite running Firmware 5.10.2
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/kindlepaperwhite.jpg)

![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/kindlepaperwhite.jpg?)

Vizio M55-C2 TV
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/viziom55-c2.jpg)
![](https://cdn.hampton.pw/hampton.pw/resources/iosWifiBug/d2/viziom55-c2.jpg)

Unlike last time the kindle was able to show the SSID without escape any of the characters. 
Sadly the Vizio was not able to show the SSID and just fell back to escaped hex.

The results from changing the SSID showed a couple things, namely devices really do not like it when they are given invalid or incomplete UTF-8 codes and lots of devices handle UTF-8 text much better than I thought they would.
It also showed that some devices fallback to 8 bit encoding formats when they see an invalid UTF-8 codepoint, there is probably some room for exploitation here like inserting escape keys.
I originally was not expecting the kindle to work at all, nor the chromebook after seeing the slew of question marks originally. Android was also found to be at handling text with invalid UTF-8 characters it just removed them as opposed to switching up formatting, falling back to hex, or erroring out with all question marks. 

Thank you to everyone who provided valuable feedback on the previous post. I have more research I plan ond doing with this in the future.

Lastly if anyone would like to run this test themselves and send me the comparison results to be added my email is `me (at) hampton (This key > not shifted) pw`.