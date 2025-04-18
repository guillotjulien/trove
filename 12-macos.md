# MacOS

## Make important keyboard shorcuts similar to Windows

Problem: Even with a Mac layer on my keyboard, I still have to deal with Linux and Windows computers on a daily basis.
So developing new muscle memory is not a solution for me right now.

Using [Karabiner-Elements](https://karabiner-elements.pqrs.org/), I can at least remap the most important stuff.
Here is my current config:
```
{
  "profiles": [
    {
      "complex_modifications": {
        "rules": [
          {
            "description": "Change home/end keys to command+arrow",
            "manipulators": [
              {
                "type": "basic",
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
                ]
              },
              {
                "type": "basic",
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
                ]
              },
              {
                "type": "basic",
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
                ]
              },
              {
                "type": "basic",
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
                ]
              }
            ]
          },
          {
            "description": "Change command+arrow to option+arrow",
            "manipulators": [
              {
                "type": "basic",
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
                "conditions": [
                  {
                    "type": "variable_unless",
                    "name": "remap_in_progress",
                    "value": 1
                  }
                ]
              },
              {
                "type": "basic",
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
                "conditions": [
                  {
                    "type": "variable_unless",
                    "name": "remap_in_progress",
                    "value": 1
                  }
                ]
              }
            ]
          }
        ]
      },
      "name": "Default profile",
      "selected": true,
      "virtual_hid_keyboard": {
        "keyboard_type_v2": "ansi"
      }
    }
  ]
}
```