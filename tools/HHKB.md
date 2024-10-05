# Keyboard and Keymap

## Why HHKB?

- Topre switches provide distinctive tactile feedback and sound; many users find it improves typing comfort

- Special layout reduces finger movement, increasing typing efficiency; Ctrl key located at Caps Lock position for easier access

- Small footprint, saves desk space; lightweight and easy to carry, suitable for switching between different work environments

- Uses premium Topre capacitive switches; durable and long-lasting

- While HHKB may require an adjustment period, many users find it significantly improves work efficiency and comfort, especially for those who code frequently.

## HHKB layout

![HHKB layout picture](/images/HHKB-Pro-2-default-layout.jpg)

Compared to traditional keyboards, there are several notable differences:

- The Control key has been placed in the original CAPS LOCK position；a good news for users who heavily rely on the Control key, such as those using emacs.
- The F1 - F12 keys, as well as the HOME, END, and INS function keys, have been removed.
- The tilde key, originally located to the left of 1, has been moved to the far right. This modification corresponds to the numbers and F1 - F12 keys.
- The Backspace key has been removed and replaced with Delete.
- The arrow keys have been removed.
- The removed keys mentioned above can be restored by using the Fn key in the bottom right corner in combination with another key.
  
## HHKB dip switches

- SW1: down + SW2: down -> HHKB layout(Unix/Linux)
- SW1: up + SW2: down -> Windows layout
- SW1: down + SW2: up -> Mac layout
- SW5:up -> backspace; SW5:down -> delete
- SW6:up -> off bluetooth sleep; SW6:down -> on bluetooth sleep

HHKB layout:

- Control key -> Caps Lock key
- Backspace key -> Delete key
- Fn+Key -> removed key

mac layout:

- right/left Meta key -> Command key
- right/left Alt key -> Option key
- media keys

windows layout:

- right/left Meta key -> right/left Windows key
- no media keys in mac layout

My preferred settings:

- Mac layout: SW1: down + SW2: up
- backspace recovery: SW5: up
- bluetooth sleep off: SW6: up

## Customized keyboard layout

### My preferred settings

- Caps Lock key -> left control key: for vim and emacs
- right Shift key -> Caps Lock key: Switch input method, especially for chinese input.

### Software vs Hardware customization

- Software: Karabiner-Elements
  - Pros:
    - more compatible: support more keyboards
    - more powerful, can implement more complicated rules
  - Cons:
    - saved settings are not easy to transfer between different machines
    - complicated, need to setup rules
- Hardware: GMK/VIA or HHKB Tool
  - Pros:
    - stored in keyboard, easy to transfer between different machines
    - set with UI, more convenient
  - Cons:
    - less compatible: only support in few keyboards

### Karabiner-Elements

- Karabiner-Elements is the tool for customizing keyboard layout, the doc is [here](https://karabiner-elements.pqrs.org/docs).

### HHKB Keymap Tool

- use dip switches to set keyboard layout
- use UI to set keyboard layout:
  - access the [website](https://happyhackingkb.com/download/#tl)
- types support keymap tools:
  - Hybrid type-s
  - Hybrid
  - Studio (not topre switch): full support GMK/VIA
