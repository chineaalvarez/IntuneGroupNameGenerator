# Structured Name Builder

A lightweight, web-based utility for generating structured names that follow customizable naming conventions.

```
<Prefix>-<Platform>-<Entity>-<Type>-<Scope>-<Purpose>
```

This tool helps administrators, developers, and teams consistently generate names for policies, groups, configurations, and other structured objects across platforms and use cases.


While the provided examples were originally created for Microsoft Intune use cases, the tool is fully adaptable and can be used to structure names for any type of entities or systems.

This includes modifying fields, values, delimiters, and logic to fit naming schemes for cloud resources, policies, configuration items, or any domain-specific objects.

---

## 🧰 Getting Started

1. **Fork** this repository into your GitHub account.
2. Go to your fork's **Settings → Pages**.
3. Under **Source**, select the `main` branch and the `/ (root)` folder, then click **Save**.
4. GitHub will publish the site and provide you with a live URL.
5. Optionally, edit the `config/schema.json` file to match your naming conventions and use cases.
6. **Commit** your changes — your customized version will be live at the GitHub Pages URL.

## 🛠️ Customization

The tool is fully configurable — you can tailor the naming structure and modify the fields to match your organization’s specific conventions. Simply adjust the `schema.json` to change field types, labels, orders, or add/remove options as needed. This allows for maximum flexibility in designing consistent and informative group names across various scenarios.

## ⚙️ Configuration Files

### `config/schema.json`

> ✏️ You can edit or add schemas in `config/schema.json`. Each schema includes its own `Fields` and `Options`, and will appear as a selectable card in the UI.

####

* **FieldType**:

  * `Static`: Hardcoded value (e.g., "INT")
  * `Predefined`: Dropdown with predefined options
  * `Text`: Free text input
* **Values**: Options available if `Predefined`
* **Delimiter**: Character(s) separating each field (typically a hyphen `-`)
* **Editable**: Fields and their delimiters can be adjusted to suit organizational needs.

### 🧩 Minimal Schema Fields Example

  This shows how to define the first few fields for a name structure.

<details>
<summary>🧾 JSON: Minimal Field Configuration</summary>

```json
{
  "Fields": [
    {
      "Service": {
        "FieldType": "Static",
        "Value": "INT",
        "Delimiter": "-"
      }
    },
    {
      "Platform": {
        "FieldType": "Predefined",
        "Values": [
          { "FriendlyName": "Windows", "Value": "WIN" },
          { "FriendlyName": "iOS", "Value": "IOS" },
          { "FriendlyName": "macOS", "Value": "MAC" },
          { "FriendlyName": "Android", "Value": "AND" }
        ],
        "Delimiter": "-"
      }
    },
    {
      "Entity": {
        "FieldType": "Predefined",
        "Values": [
          { "FriendlyName": "Devices", "Value": "D" },
          { "FriendlyName": "Users", "Value": "U" }
        ],
        "Delimiter": "-"
      }
    }
  ]
}
```

</details>

### 🧱 SubFields Mechanism

  SubFields allow you to define nested field logic, where a selected value from one field triggers the appearance of another dependent field. This is useful for scenarios where you want additional granularity based on a parent field choice.

  Each `SubField` is declared inside a value's definition and follows the same structure as a top-level field.

For example, under the `Purpose` field:

<details>
<summary>🧾 JSON: SubFields with HideFields</summary>

```json
{
  "Purpose": {
    "FieldType": "Predefined",
    "Values": [
      {
        "FriendlyName": "Applications",
        "Value": "APPS",
        "SubFields": [
          {
            "Intent": {
              "FieldType": "Predefined",
              "Values": [
                { "FriendlyName": "Required", "Value": "REQUIRED" },
                { "FriendlyName": "Uninstall", "Value": "UNINSTALL" },
                { "FriendlyName": "Available", "Value": "AVAILABLE" }
              ],
              "Delimiter": "_"
            }
          },
          {
            "AppName": {
              "FieldType": "FreeText",
              "Delimiter": "_"
            }
          },
          {
            "Version": {
              "FieldType": "FreeText",
              "Delimiter": "_"
            }
          }
        ],
        "HideFields": ["Scope"]
      }
    ],
    "Delimiter": "-"
  }
}
```

</details>

In this case, selecting "Applications" in the Purpose field will dynamically reveal additional fields — Intent, AppName, and Version — allowing for more specific naming.

`HideField` is used to hide fields from the UI when they are no longer needed due to the presence of SubFields.

### ⚙️ Options

  Controls the **behavior and formatting rules** of the generator:

* **WhitespaceReplacement**:

  * Replaces spaces with a specified character (e.g., `_`)
  * `enabled`: `true` to activate it
  * `editable`: `true` to allow user changes
  * `config.Character.value`: The character to use

* **MaxLength**:

  * Limits the overall length of the generated group name
  * `enabled`: `true` or `false`
  * `editable`: `false` if you want it fixed
  * `config.Length.value`: Maximum number of characters

* **Uppercase**:

  * Converts the full group name to uppercase
  * `enabled`: Set to `true` to force uppercase
  * `editable`: Allows the user to turn it on or off

* **IncludeTimestamp**:

  * Appends a timestamp at the end
  * `enabled`: Controls activation
  * `editable`: Allows toggling
  * `config.Format.value`: Format string like `DDMMYYYY`

<details>
<summary>🧾 JSON: Options Configuration</summary>

```json
{
  "WhitespaceReplacement": {
    "FriendlyName": "Whitespace character",
    "enabled": true,
    "editable": true,
    "config": {
      "Character": {
        "value": "_",
        "type": "Text"
      }
    }
  },
  "MaxLength": {
    "FriendlyName": "Max length",
    "enabled": true,
    "editable": false,
    "config": {
      "Length": {
        "value": 50,
        "type": "Integer"
      }
    }
  },
  "Uppercase": {
    "FriendlyName": "Convert to uppercase",
    "enabled": false,
    "editable": true
  },
  "IncludeTimestamp": {
    "FriendlyName": "Timestamp format",
    "enabled": false,
    "editable": false,
    "config": {
      "Format": {
        "value": "DDMMYYYY",
        "type": "Text"
      }
    }
  }
}
```

</details>

---

<details>
  <summary>🧾 JSON: Full Schema Example</summary>

  ##### JSON Snippet: Full Schema Example

  ```json
  {
    "Name": "Intune Groups",
    "Fields": [
      {
        "Service": {
          "FieldType": "Static",
          "Value": "INT",
          "Delimiter": "-"
        }
      },
      {
        "Platform": {
          "FieldType": "Predefined",
          "Values": [
            { "FriendlyName": "Windows", "Value": "WIN" },
            { "FriendlyName": "iOS", "Value": "IOS" },
            { "FriendlyName": "macOS", "Value": "MAC" },
            { "FriendlyName": "Android", "Value": "AND" }
          ],
          "Delimiter": "-"
        }
      },
      {
        "Entity": {
          "FieldType": "Predefined",
          "Values": [
            { "FriendlyName": "Devices", "Value": "D" },
            { "FriendlyName": "Users", "Value": "U" }
          ],
          "Delimiter": "-"
        }
      },
      {
        "Purpose": {
          "FieldType": "Predefined",
          "Values": [
            {
              "FriendlyName": "Applications",
              "Value": "APPS",
              "SubFields": [
                {
                  "Intent": {
                    "FieldType": "Predefined",
                    "Values": [
                      { "FriendlyName": "Required", "Value": "REQUIRED" },
                      { "FriendlyName": "Uninstall", "Value": "UNINSTALL" },
                      { "FriendlyName": "Available", "Value": "AVAILABLE" }
                    ],
                    "Delimiter": "_"
                  }
                },
                {
                  "AppName": {
                    "FieldType": "FreeText",
                    "Delimiter": "_"
                  }
                },
                {
                  "Version": {
                    "FieldType": "FreeText",
                    "Delimiter": "_"
                  }
                }
              ],
              "HideFields": ["Scope"]
            },
            { "FriendlyName": "Security", "Value": "SECURITY" },
            { "FriendlyName": "Compliance", "Value": "COMPLIANCE" },
            { "FriendlyName": "Updates", "Value": "UPDATE" },
            { "FriendlyName": "Onboarding", "Value": "ONBOARDING" },
            { "FriendlyName": "Deployment", "Value": "DEPLOYMENT" },
            { "FriendlyName": "Monitoring", "Value": "MONITORING" },
            { "FriendlyName": "Testing", "Value": "TESTING" }
          ],
          "Delimiter": "-"
        }
      },
      {
        "Scope": {
          "FieldType": "FreeText",
          "Delimiter": "-"
        }
      }
    ],
    "Options": {
      "WhitespaceReplacement": {
        "FriendlyName": "Whitespace character",
        "enabled": true,
        "editable": true,
        "config": {
          "Character": {
            "value": "_",
            "type": "Text"
          }
        }
      },
      "MaxLength": {
        "FriendlyName": "Max length",
        "enabled": true,
        "editable": false,
        "config": {
          "Length": {
            "value": 50,
            "type": "Integer"
          }
        }
      },
      "Uppercase": {
        "FriendlyName": "Convert to uppercase",
        "enabled": false,
        "editable": true
      },
      "IncludeTimestamp": {
        "FriendlyName": "Timestamp format",
        "enabled": false,
        "editable": false,
        "config": {
          "Format": {
            "value": "DDMMYYYY",
            "type": "Text"
          }
        }
      }
    }
  }
  ```

</details>

## 📋 Example Group Name

If you select:

* Platform: `WIN`
* Entity: `D`
* Type: `Dyn`
* Scope: `Developers`
* Purpose: `Apps`

The generated group name will be:

```
<Prefix>-WIN-D-Dyn-Developers-Apps
```

If whitespace replacement and uppercase rules are enabled, an input like:

```
All Kiosk Devices
```

would result in:

```
<Prefix>-WIN-D-Dyn-All_Kiosk_Devices-Apps
```

---

## 🚀 Live Demo

You can try the Structured Name Builder right now:

🔗 [Open Live Demo](https://chineaalvarez.github.io/Structure-Name-Builder/)

This is hosted on GitHub Pages using the same code in this repository. You can fork and customize your own version anytime.

---

## 🖼️ Screenshot
![image](https://github.com/user-attachments/assets/55a888b5-d0b6-4cdc-9942-43022ed61123)

---

## 📄 License

MIT License – open-source and free to use, modify, and distribute.
