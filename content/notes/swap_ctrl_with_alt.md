+++
title = "Swap CTRL and ALT keys on arch linux"
date = ""
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["linux", "keyboard shortcuts", "touch typing"]
description = "How to CTRL and ALT keys using "
showFullContent = false
readingTime = false
hideComments = false
color = "orange" #color from the theme settings
Toc = false
+++

Using macos keyboard for the first time after windows/linux is not frictional at first but after you get used to it you realize that it does one thing better. In macos most keyboard shortcuts use CMD instead of CTRL key which is better placed. CMD can be pressed by your thumbs without moving your fingers from touch typing position. I don't know you you're supposed to press CTRL with touch typing but I had the habit of using my thumbs.

Recently I moved back to a linux laptop and decided to swap CTRL with ALT keys along with CAPS and the ESC key for vim.
Apparently there's no way to do this in XFCE using UI, you have to use some additional tools. Xmodmap comes up first in search results, I set it up with the following init file.

```bash {title=".Xmodmap"}
clear control
clear mod1
clear Lock
keycode 37 = Alt_L
keycode 64 = Control_L
keycode 105 = Alt_R
keycode 108 = Control_R
keycode 66 = Escape
keycode 9 = Caps_Lock
add control = Control_L
add control = Control_R
add mod1 = Alt_L
add mod1 = Alt_R
```

This requires some change in navigation shortcuts such as ALT + TAB, it can be changed to CTRL + TAB for macos like navigation.

Now the using CTRL + C and CTRL + V is much easier and quicker.
