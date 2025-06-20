# Intune Group Name Generator

This is a simple web-based tool to help generate standardized Microsoft Intune group names following this naming convention:

INT-<Platform>-<Entity>-<Type>-<Scope     | Who the group applies to (membership scope) | `All`, `WSLUsers`, `Developers`, `Interns`, `KioskDevices`, `SharedPCs`, `POS_Terminals` | Field     | Description                                | Example values                          |
|-----------|--------------------------------------------|------------------------------------------|
| Platform  | Operating system                           | `WIN`, `MAC`, `IOS`, `AND`              |
| Entity    | Group entity type                          | `D` (Devices), `U` (Users)              |
| Type      | Group type                                 | `Dyn`, `Sta`                            |
| Scope     | Who the group applies to (membership scope) | `All`, `WSLUsers`, `Developers`, `Interns` |
| Purpose   | Functional use of the group                | `Apps`, `Security`, `Compliance`        |

Spaces in any input are automatically replaced with underscores (`_`).

## 📦 How to Use

1. Open `index.html` in a browser.
2. Fill out the form fields.
3. The tool will automatically generate the group name.

## 🌐 GitHub Pages

To publish this tool via GitHub Pages:

1. Create a GitHub repository (e.g., `intune-group-generator`).
2. Upload the `index.html` file.
3. Go to **Settings → Pages**, set Source to `main` branch, `/ (root)` folder.
4. Your tool will be accessible at:

```
https://<your-username>.github.io/<your-repo-name>/
```

## 📸 Screenshot

*Optional: Add a screenshot and name it `screenshot.png`*

## 📃 License

MIT License. Open-source and free to use.
