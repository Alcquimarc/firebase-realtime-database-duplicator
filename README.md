# Firebase Realtime Database Duplicator

A browser-based utility for **copying, migrating, backing up, and consolidating data between Firebase Realtime Database instances** using the Firebase Realtime Database REST API.

The tool runs entirely in the browser and provides a simple interface for copying data from one Firebase Realtime Database location to another.

## 🚀 Live Demo

### Primary Website

**https://alcquimarc.is-best.net/firebase-migrator/**

### GitHub Pages

**https://alcquimarc.github.io/firebase-realtime-database-duplicator/**

Both links provide access to the Firebase Realtime Database Duplicator web application.

## ✨ Features

* **Source database is read-only**
  The `Copy From` database is accessed using HTTP `GET` requests only. The tool never writes to or deletes data from the source database.

* **Destination database writing**
  Data can be written to the destination using:

  * **Merge** — uses `PATCH` to add or update data while preserving destination keys that are not included in the source.
  * **Replace** — uses `PUT` to completely overwrite the selected destination path.

* **Preview source data**
  Load and inspect Firebase data before performing a copy operation.

* **JSON backup**
  Download the currently loaded source data as a local `.json` backup.

* **Path-based migration**
  Copy the entire database or a specific Firebase Realtime Database node.

* **Optional authentication**
  Supports an optional authentication token for databases that require authenticated access.

* **Firebase Rules assistance**
  The interface can inspect pasted Firebase Realtime Database Rules and indicate whether authentication may be required.

* **Operation logging**
  Displays the progress and result of source reads and destination writes.

* **No backend required**
  The application is a standalone HTML/JavaScript application and performs its operations directly from the browser.

The application reads the source database, allows the data to be previewed or downloaded as a backup, and then writes it to the destination using either Merge or Replace mode.

## 🗄️ How It Works

```text
Firebase Realtime Database
        │
        │ GET
        ▼
   Copy From
    (Source)
        │
        │ Preview / Backup
        ▼
   Browser Memory
        │
        │ PATCH or PUT
        ▼
    Copy To
  (Destination)
```

The source database is read first. The data can then be previewed or downloaded as a JSON backup before being written to the destination database.

## 1. Configure the Source

Enter the Firebase Realtime Database URL under **Copy From**.

You may also specify:

* A database path
* An authentication token
* Firebase Realtime Database Rules

The source database is only read and is never modified.

## 2. Preview the Data

Click **Preview Source Data** to retrieve the selected data using the Firebase REST API.

The application displays:

* Retrieved JSON data
* Data type
* Number of top-level keys/items
* Approximate data size

After the source has been loaded, you can also download the data as a local JSON backup.

## 3. Configure the Destination

Enter the Firebase Realtime Database URL under **Copy To**.

You can optionally specify:

* A destination path
* An authentication token

Destination paths are useful when consolidating multiple Firebase databases into a single project.

## 4. Select the Write Mode

### Merge

Uses Firebase's `PATCH` method.

This adds or updates the source's top-level keys while leaving other destination keys unchanged.

```text
PATCH /destination.json
```

### Replace

Uses Firebase's `PUT` method.

This completely replaces the selected destination path with the source data.

```text
PUT /destination.json
```

Replace mode includes a confirmation prompt because existing destination data can be overwritten.

## 💡 Example Use Cases

### Database Migration

Move data from an old Firebase project to a new Firebase project.

```text
Old Firebase Project
        │
        ▼
Firebase Realtime Database Duplicator
        │
        ▼
New Firebase Project
```

### Database Consolidation

Combine data from several Firebase projects into a single project using different destination paths.

```text
Project A ──► legacy/projectA
Project B ──► legacy/projectB
Project C ──► legacy/projectC
```

The application supports destination paths specifically for this type of consolidation workflow.

### Database Backup

Preview the source database and download its contents as a JSON file before migration.

### Project Decommissioning

Copy important Realtime Database data to another Firebase project before replacing or decommissioning the original project.

## 🛠️ Requirements

You only need:

* A modern web browser
* Access to the source Firebase Realtime Database
* Access to the destination Firebase Realtime Database
* Appropriate Firebase Realtime Database permissions

The application does not require:

* PHP
* Node.js
* MySQL
* A backend server

## 💻 Running Locally

Clone the repository:

```bash
git clone https://github.com/alcquimarc/firebase-realtime-database-duplicator.git
```

Open the project directory:

```bash
cd firebase-realtime-database-duplicator
```

Then open:

```text
index.html
```

in your browser.

The application can also be deployed to any static web hosting provider.

## 🌐 Deployment

This project is compatible with static web hosting because it consists of HTML, CSS, and JavaScript.

Current deployments:

| Deployment      | URL                                                                 |
| --------------- | ------------------------------------------------------------------- |
| Primary Website | https://alcquimarc.is-best.net/firebase-migrator/                   |
| GitHub Pages    | https://alcquimarc.github.io/firebase-realtime-database-duplicator/ |

## 🔥 Firebase REST API

The application constructs Firebase Realtime Database REST endpoints using the database URL, optional path, and optional authentication token.

The application uses:

```text
GET    → Read source data
PATCH  → Merge data into destination
PUT    → Replace destination data
```

The requests are performed directly in the browser using JavaScript `fetch()`.

## 🔐 Security Considerations

**Use this tool carefully with production databases.**

The application communicates directly with Firebase from the user's browser. Access is therefore controlled by Firebase Realtime Database Rules and any authentication credentials supplied by the user.

### Important

* Never publish database secrets or authentication tokens in the source code.
* Do not commit credentials to GitHub.
* Only provide authentication credentials to trusted users.
* Verify the destination database before using **Replace** mode.
* Create a backup before performing a migration.
* Use appropriate Firebase security rules.

The application masks authentication values in displayed endpoints and requires confirmation before Replace mode performs a write.

## ⚠️ Limitations

This project is intended for typical project-sized Firebase Realtime Database operations.

For very large databases, Firebase's dedicated export/import tools may be more appropriate than transferring the complete dataset through a browser.

The application itself identifies browser-based operation as being suited to typical project-sized data.

## 📁 Project Structure

```text
firebase-realtime-database-duplicator/
│
├── index.html
├── README.md
└── LICENSE
```

The application is currently implemented as a standalone HTML file containing the interface, CSS, and JavaScript functionality.

## 👨‍💻 Author

**Brian-Alcquimarc S. Lawama**

Facebook:
https://facebook.com/alcquimarc

## 📄 License

This project is licensed under the **MIT License**.

See the [LICENSE](LICENSE) file for the complete license text.

## ⚖️ Disclaimer

This tool is intended for legitimate database administration, migration, backup, testing, and development purposes.

Users are responsible for ensuring that they have authorization to access, copy, migrate, or modify the Firebase Realtime Database instances used with this application.

Always verify Firebase security rules, authentication credentials, source paths, and destination paths before performing a migration.
