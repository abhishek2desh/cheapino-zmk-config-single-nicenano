Muscle Memory 36key layout for Cheapino V2.
Original author for layout https://gitlab.com/getreu
More details about the layout is here https://blog.getreu.net/20250826-muscle-memory-friendly-home-row-mods/

Status:
Encoder not supported.
USB and BLUETOOTH both supported on nicenano v2.
layout has been ported from Totem which is a 38 Key layout. I might have missed couple of combos or other missed behaviors, and other bugs, use at your own risk.


Hardware: Nice Nano v2 
Important note. don't add pin headers to vcc pin on control board [near RST pin]. This pin corresponds to encoder on the shield. You can jump it on the shield from Right pin1 to D2 of Pro Micro.

Power on switch:
I have opted for soft off feature from zmk so no dedicated power on switch. Instead using reset button on the controller itself to awaken the board.

Battery:
For battery I have added a slim profile iphone se3 replacement (3500 mAH) which was lying around. I dropped the proprietary protection board and connector for a general purpose li ion protection board.

On the Nice Nano, I have shorted the boosted charging pins. so max current is 300 mAH.

