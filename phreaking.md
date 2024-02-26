---
layout: page
title: Phreaking
hide: true
permalink: /phreaking/
---

PhilTel is "phreaking friendly," meaning we encourage users to both experiment with and explore the telephone network. 

## Overview

[Phone phreaking](https://en.wikipedia.org/wiki/Phreaking){:target="_blank"} started in the 1950's when telephone users would spend time dialing different numbers within the telephone network in an attempt to figure out how the system worked. In 1971, phreaking entered the mainstream when Esquire magazine published ["Secrets of the Little Blue Box,"](http://www.thestacksreader.com/secrets-of-the-blue-box-ron-rosenbaum-steve-jobs-influence/){:target="_blank"} an article that featured [Joybubbles](https://en.wikipedia.org/wiki/Joybubbles){:target="_blank"} (then known as Joe Engressia) and [John Draper](https://en.wikipedia.org/wiki/John_Draper){:target="_blank"} (also known as Captain Crunch) with regards to Joybubbles' discovery of how a 2600 Hz tone could be used to disconnect long-distance calls and allow the caller to then call anywhere for free. Draper would later build a low-cost electronic device known as a "blue box" that could generate this tone on demand and also allow the user to easily enter multi-frequency tones so a new phone number could be dialed.

{% raw %}<p><center><a href="/assets/img/woz_blue_box.jpg"><img style="width: 80%; max-width: 500px; display: block; margin: 0 auto; border 0" src="/assets/img/woz_blue_box-sm.jpg"></a><figquote>This blue box was created by Steve Wozniak before he went on to co-found Apple Computer. Photo by Maksym Kozlenko.</figquote></center></p>{% endraw %}  

Other phreaking boxes were also popular throughout the 1960s through the early 2000s including the "red box" which can emulate the tones of coins being accepted by payphones. This device made the payphone believe the user had deposited coins when they had not, allowing free calls to be made. By the 1980s, phone phreaking had a lot of overlap with and influence on the budding computer hacking movement, which extended the spirit of exploration to computer networks instead of phone systems.

{% raw %}<p><center><a href="/assets/img/redbox.jpg"><img style="width: 80%; max-width: 500px; display: block; margin: 0 auto; border 0" src="/assets/img/redbox-sm.jpg"></a><figquote>Red boxes were commonly made by replacing the crystal in a Radio Shack pocket tone dialer. The new crystal changed the frequency of the tones the device would produce, allowing the device to emit coin tones. Photo by bc219.</figquote></center></p>{% endraw %}  

These days, advances in technology have closed many of the loopholes that were previously exploited for free phone calls. However, the phone system is larger than ever and curious people can still pick up a phone and dial different numbers to explore the system and discover how it works from the inside out.

{% raw %}<p><center><a href="/assets/img/wargames.jpg"><img style="width: 80%; max-width: 500px; display: block; margin: 0 auto; border 0" src="/assets/img/wargames.jpg"></a><figquote>You may have even seen examples of phone phreaking in popular films without even knowing it! Main characters in WarGames (1983), Sneakers (1992), and Hackers (1995) perform phone phreaking activities on screen!</figquote></center></p>{% endraw %}  

## Red-Boxing from a PhilTel phone

[PhreakNet](https://portal.phreaknet.org/directory){:target="_blank"} has several numbers you can dial that will give you a simulated payphone trunk that will respond to coin (red box) tones.

### Acquiring a Red Box

Red boxes can be built by modifying pocket tone dialers, assembled by hand from electronic components, or simulated via audio files and online apps. Check out [phonelosers.org dummies guide to red boxing ](http://www.phonelosers.org/redbox/){:target="_blank"} for downloads of coin tone audio files. If you feel like building a red box, I once recommend building [Don Froula's Arduino-based Multi-Mode Blue Box](https://www.instructables.com/Arduino-12-mode-Blue-Box-Introduction/){:target="_blank"}, which can playback blue box tones, red box tones, and many others! You can also try building your own red box by [modifying a pocket tone dialer](https://phonelosers.com/redbox/tonedialer/){:target="_blank"}.  
  
For most people, using the online PhreakNet webapp's [1-SLOT COIN DENOMINATION TONES](https://phreaknet.org/bluebox/){:target="_blank"} will work for most people on their cell phones.  

### How to Red-Box
  
While Red-boxing once required an actual coin line from the telephone company, PhilTel phones can now simulate these lines for red-boxing for calls to PhreakNet numbers!  
  
To red-box, do the following,  
1) Pick up the handset and dial _*11_ to drop into a pre-pay (coin first) coin line. This line simulates a CoinZone coin trunk as would normally be provided to the payphone from a phone company. You should hear a high-pitched whine after dialing this number.  
2) Now is time to red-box! Using your red box of choice, play the audio for the denomination of _10 cents_ into the handset mouthpiece. You should hear two chirps from your red box and you should then hear a dialtone from the phone. If you do not hear a dialtone, try the _10 cents_ audio again.  
3) Now that you have dialtone, try dialing a 7-digit PhreakNet number like _767-2676_. You will hear sounds from the phone when entering each digit, just like you would have if dialing back in the day! After dialing the full number, it should connect. If you dialed _767-2676_ it will provide the time and temperature.  
  
There are many types of coin lines! Feel free to experiment with lines from the following list which may have different behavior (including prompting for more money after the initial 10-dent desposit):  
  
* *11 - Pre-Pay (Coin First) coin line, CoinZone coin trunk
* *12 - Post-Pay (WECo) coin line (CoinZone coin trunk)
* *13 - Dial-tone first coin line (CoinZone coin trunk)
* *14 - Dial-tone first coin line (ACTS coin trunk)
* *15 - Semi-Dial-tone first coin line (CoinZone coin trunk)
* *16 - Dial-tone first coin line (Nortel TOPS ACTS trunk)
  
<iframe title="PhilTel Payphone Red Boxing" src="https://diode.zone/videos/embed/5ef97335-b8bc-4be6-ac5c-b3aeb2e00354" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups" width="560" height="315" frameborder="0"></iframe>
  
## Blue-Boxing from a PhilTel phone

PhreakNet's native 2600 trunks are undergoing maintenance, but you can still use a PhilTel phone to blue-box via [ProjectMF's switch](https://projectmf.org/){:target="_blank"}.  
  
### Acquiring a Blue Box

Blue boxes are not readily available commercially, though they can be built from electronic components or simulated via a computer or mobile phone. Check out [PhreakNet's Virtual Blue Box](https://phreaknet.org/bluebox/){:target="_blank"} that will run in a web browser. If you feel like building a blue box, I recommend [Don Froula's Arduino-based Multi-Mode Blue Box](https://www.instructables.com/Arduino-12-mode-Blue-Box-Introduction/){:target="_blank"}, which can playback blue box tones, red box tones, and many others!  

For most people, using the online PhreakNet [Ultimate Online "Phreak Box"](https://phreaknet.org/bluebox/){:target="_blank"} will work for most people on their cell phones. 
  
### How to Blue-Box
  
We will be blue-boxing directly from ProjectMF's switch within PhreakNet. Keep in mind that blue-boxing does not always work the first time, and this is true-to-form as it did not always work first time when it was at its peak use. Having issues is totally normal and expected.  
  
To blue-box, do the following,  
1) Pick up the handset and dial _1-400-762-2601_. You should hear some tones in the background folloed by ringing.  
2) During this ringing, play a _2600 Hz_ tone into the mouthpiece of the handset. The ringing should stop and you should hear a _wink_ which sounds a but like tapping a knife against the rim of a glass a few times. If you do not hear a wink, try playing the _2600 Hz_ tone again. The line can be sensitive, so trying higher/lower volume can give different levels of success.  
3) After the wink, you have about five seconds to start dialing another number, so quickly dial _KP_ (key pulse) so signify the start of a new call.  
4) Next, dial _112_ (MF digits) to signify the number we want to dial. This is a 3-digit extension on the ProjectMF switch, and other extensions can be found [here](https://projectmf.org/intro.html){:target="_blank"} if you want to experiment with other destinations.
5) Finally, dial _ST_ (STart). After this, you should hear the digits dialing in the background and the call will connect to a recording describing the phreaking technique of "Tandem Stacking."
  
<iframe title="PhilTel Payphone Blue Boxing" src="https://diode.zone/videos/embed/7e8a7961-4432-4e0e-a7dc-db79e5fbaf93" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups" width="560" height="315" frameborder="0"></iframe>



