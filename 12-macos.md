# MacOS

## Make important keyboard shorcuts similar to Windows

Problem: Even with a Mac layer on my keyboard, I still have to deal with Linux and Windows computers on a daily basis.
So developing new muscle memory is not a solution for me right now.

Using [Karabiner-Elements](https://karabiner-elements.pqrs.org/), I can at least remap the most important stuff.
Here is my current config:
```
{
  "manipulators": [
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.googlecode\\.iterm2$"
          ],
          "type": "frontmost_application_unless"
        }
      ],
      "description": "Remap Home key to Command+Left Arrow (unless in iTerm2)",
      "from": {
        "key_code": "home",
        "modifiers": {
          "optional": [
            "any"
          ]
        }
      },
      "to": [
        {
          "set_variable": {
            "name": "remap_in_progress",
            "value": 1
          }
        },
        {
          "key_code": "left_arrow",
          "modifiers": [
            "left_command"
          ]
        }
      ],
      "to_after_key_up": [
        {
          "set_variable": {
            "name": "remap_in_progress",
            "value": 0
          }
        }
      ],
      "type": "basic"
    },
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.googlecode\\.iterm2$"
          ],
          "type": "frontmost_application_unless"
        }
      ],
      "description": "Remap Home+Shift key to Command+Shift+Left Arrow (unless in iTerm2)",
      "from": {
        "key_code": "home",
        "modifiers": {
          "mandatory": [
            "left_shift"
          ],
          "optional": [
            "any"
          ]
        }
      },
      "to": [
        {
          "set_variable": {
            "name": "remap_in_progress",
            "value": 1
          }
        },
        {
          "key_code": "left_arrow",
          "modifiers": [
            "left_command",
            "left_shift"
          ]
        }
      ],
      "to_after_key_up": [
        {
          "set_variable": {
            "name": "remap_in_progress",
            "value": 0
          }
        }
      ],
      "type": "basic"
    },
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.googlecode\\.iterm2$"
          ],
          "type": "frontmost_application_unless"
        }
      ],
      "description": "Remap End key to Command+Right Arrow (unless in iTerm2)",
      "from": {
        "key_code": "end",
        "modifiers": {
          "optional": [
            "any"
          ]
        }
      },
      "to": [
        {
          "set_variable": {
            "name": "remap_in_progress",
            "value": 1
          }
        },
        {
          "key_code": "right_arrow",
          "modifiers": [
            "left_command"
          ]
        }
      ],
      "to_after_key_up": [
        {
          "set_variable": {
            "name": "remap_in_progress",
            "value": 0
          }
        }
      ],
      "type": "basic"
    },
    {
      "conditions": [
        {
          "bundle_identifiers": [
            "^com\\.googlecode\\.iterm2$"
          ],
          "type": "frontmost_application_unless"
        }
      ],
      "description": "Remap End+Shift key to Command+Shift+Right Arrow (unless in iTerm2)",
      "from": {
        "key_code": "end",
        "modifiers": {
          "mandatory": [
            "left_shift"
          ],
          "optional": [
            "any"
          ]
        }
      },
      "to": [
        {
          "set_variable": {
            "name": "remap_in_progress",
            "value": 1
          }
        },
        {
          "key_code": "right_arrow",
          "modifiers": [
            "left_command",
            "left_shift"
          ]
        }
      ],
      "to_after_key_up": [
        {
          "set_variable": {
            "name": "remap_in_progress",
            "value": 0
          }
        }
      ],
      "type": "basic"
    },
    {
      "description": "Remap Command+Left Arrow to Option+Left Arrow",
      "conditions": [
        {
          "name": "remap_in_progress",
          "type": "variable_unless",
          "value": 1
        }
      ],
      "from": {
        "key_code": "left_arrow",
        "modifiers": {
          "mandatory": [
            "command"
          ],
          "optional": [
            "any"
          ]
        }
      },
      "to": [
        {
          "key_code": "left_arrow",
          "modifiers": [
            "left_option"
          ]
        }
      ],
      "type": "basic"
    },
    {
      "description": "Remap Command+Right Arrow to Option+Right Arrow",
      "conditions": [
        {
          "name": "remap_in_progress",
          "type": "variable_unless",
          "value": 1
        }
      ],
      "from": {
        "key_code": "right_arrow",
        "modifiers": {
          "mandatory": [
            "command"
          ],
          "optional": [
            "any"
          ]
        }
      },
      "to": [
        {
          "key_code": "right_arrow",
          "modifiers": [
            "left_option"
          ]
        }
      ],
      "type": "basic"
    }
  ]
}
```