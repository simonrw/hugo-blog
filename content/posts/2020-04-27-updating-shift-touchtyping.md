---
title: "Updating Shift Touchtyping"
date: "2020-04-27T22:58:51+01:00"
summary: I was pointed to Steve Losh's blog post about helping improve touch typing. Steve used Karabiner Elements (then KeyRemap4MacBook) to disable the incorrect shift key combinations (i.e. for capital "P" the left shift must be used).  His example code is now out of date, and the equivalent code must be used.
---

I was pointed to Steve Losh's [blog post](https://stevelosh.com/blog/2012/10/a-modern-space-cadet/#s15-better-shifting) about helping improve touch typing by the very helpful and friendly Chris Toomey. Steve used Karabiner Elements (then KeyRemap4MacBook) to disable the "incorrect" shift key combinations (i.e. for capital "P" the left shift _must_ be used).  

His example code is now out of date, and the equivalent code must be used. I put this file under `~/.config/karabiner/assets/complex_modifications/touchtype.json`:

```json
{
  "title": "Learn to touch type",
  "rules": [
    {
      "description": "Disable incorrect shift key",
      "manipulators": [
        {
          "type": "basic",
          "from": {
            "key_code": "q",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "w",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "e",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "r",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "t",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "a",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "s",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "d",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "f",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "g",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "z",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "x",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "c",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "v",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "1",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "2",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "3",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "4",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "5",
            "modifiers": {
              "mandatory": [
                "left_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "y",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "u",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "i",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "o",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "p",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "h",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "j",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "k",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "l",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "semicolon",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "b",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "n",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "m",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "comma",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "period",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "slash",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "6",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "7",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "8",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "9",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "0",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "open_bracket",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "close_bracket",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        },
        {
          "type": "basic",
          "from": {
            "key_code": "quote",
            "modifiers": {
              "mandatory": [
                "right_shift"
              ]
            }
          }
        }
      ]
    }
  ]
}
```

