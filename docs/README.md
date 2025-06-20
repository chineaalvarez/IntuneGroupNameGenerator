# Intune Group Name Generator

A lightweight, web-based utility for generating Microsoft Intune group names that conform to a consistent naming convention:

```
INT-<Platform>-<Entity>-<Type>-<Scope>-<Purpose>
```

This naming structure helps IT administrators maintain clarity and uniformity when managing dynamic/static groups for devices and users in Microsoft Intune.

---

## 🚀 How It Works

1. Open `index.html` in any modern browser.
2. Use the form fields to select or enter values for:

   * **Platform** (e.g. WIN, MAC, IOS, AND)
   * **Entity** (D for Devices, U for Users)
   * **Type** (Dyn for Dynamic, Sta for Static)
   * **Scope** (e.g. All, Interns, Developers)
   * **Purpose** (e.g. Apps, Security)
3. The tool automatically generates the final group name based on your input.
4. The output updates in real-time, and you can copy the final name for use in Intune.
5. Most fields and their delimiters are fully editable through the schema configuration.

---

## ⚙️ Configuration Files

### `config/schema.json`

####

* **FieldType**:

  * `Static`: Hardcoded value (e.g., "INT")
  * `Predefined`: Dropdown with predefined options
  * `Text`: Free text input
* **Values**: Options available if `Predefined`
* **Delimiter**: Character(s) separating each field (typically a hyphen `-`)
* **Editable**: Fields and their delimiters can be adjusted to suit organizational needs.

#### Example:

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

#### SubFields Mechanism

SubFields allow you to define nested field logic, where a selected value from one field triggers the appearance of another dependent field. This is useful for scenarios where you want additional granularity based on a parent field choice.

Each `SubField` is declared inside a value's definition and follows the same structure as a top-level field.

For example, under the `Purpose` field:

```
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
                { "FriendlyName": "Required", "Value": "REQ" },
                { "FriendlyName": "Available", "Value": "AVL" },
                { "FriendlyName": "Uninstall", "Value": "UNINST" }
              ],
              "Delimiter": "-"
            }
          }
        ]
      }
    ]
  }
}
```

In this case, selecting `Applications` in the Purpose field prompts the UI to show a second dropdown for `Intent`, allowing for an extended group name like:

```
INT-WIN-D-Dyn-Developers-Apps-REQ
```

The delimiter for each SubField can be customized independently.

#### `HideField` is used to hide fields from the UI when they are no longer needed due to the presence of SubFields.

### `config/options.json`

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

#### Example:

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

---

## 📋 Example Group Name

If you select:

* Platform: `WIN`
* Entity: `D`
* Type: `Dyn`
* Scope: `Developers`
* Purpose: `Apps`

The generated group name will be:

```
INT-WIN-D-Dyn-Developers-Apps
```

If whitespace replacement and uppercase rules are enabled, an input like:

```
All Kiosk Devices
```

would result in:

```
INT-WIN-D-Dyn-All_Kiosk_Devices-Apps
```

---

## 🌐 Hosting (Optional)

To host it via GitHub Pages:

1. Push the project to a GitHub repository.
2. Go to **Settings → Pages** and set the source to the root of the `main` branch.
3. The tool will be available at:

```
https://<your-username>.github.io/<your-repo-name>/
```

---

## 🖼️ Screenshot

---

## 📄 License

MIT License – open-source and free to use, modify, and distribute.
