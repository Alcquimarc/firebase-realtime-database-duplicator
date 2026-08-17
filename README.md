# firebase-realtime-database-duplicator
A browser-based tool for copying, migrating, backing up, and consolidating data between Firebase Realtime Database instances using the REST API.

# Firebase Realtime Database Duplicator

A browser-based utility for **copying, migrating, backing up, and consolidating data between Firebase Realtime Database instances** using the Firebase Realtime Database REST API.

The tool runs entirely in the browser and provides a simple interface for copying data from one Firebase Realtime Database location to another.

## Features

- **Source database is read-only**  
  The `Copy From` database is accessed using HTTP `GET` requests only. The tool never writes to or deletes data from the source database.

- **Destination database writing**  
  Data can be written to the destination using:
  - **Merge** — uses `PATCH` to add or update data while preserving destination keys that are not included in the source.
  - **Replace** — uses `PUT` to completely overwrite the selected destination path.

- **Preview source data**  
  Load and inspect Firebase data before performing a copy operation.

- **JSON backup**  
  Download the currently loaded source data as a local `.json` backup.

- **Path-based migration**  
  Copy the entire database or a specific Firebase Realtime Database node.

- **Optional authentication**  
  Supports an optional authentication token for databases that require authenticated access.

- **Firebase Rules assistance**  
  The interface can inspect pasted Firebase Realtime Database Rules and indicate whether authentication may be required.

- **Operation logging**  
  Displays the progress and result of source reads and destination writes.

- **No backend required**  
  The application is a standalone HTML/JavaScript application and performs its operations directly from the browser.

The application's interface explicitly describes the workflow as reading the source, optionally downloading a backup, then writing to the destination using Merge or Replace.

## How It Works

The migration process follows this sequence:

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

### 1. Configure the source

Enter the Firebase Realtime Database URL under **Copy From**.

You may also specify:

- A database path
- An authentication token
- Firebase Realtime Database Rules

The source database is only read and is never modified.

### 2. Preview the data

Click **Preview Source Data** to retrieve the selected data using the Firebase REST API.

The application displays:

- The retrieved JSON data
- Data type
- Number of top-level keys/items
- Approximate data size

A local JSON backup can also be downloaded after the data has been loaded. 
### 3. Configure the destination

Enter the Firebase Realtime Database URL under **Copy To**.

You can optionally specify a destination path and authentication token.

### 4. Select the write mode

#### Merge

Uses Firebase's `PATCH` method.

This adds or updates the source's top-level keys while leaving other destination keys unchanged.

```text
PATCH /destination.json
```

#### Replace

Uses Firebase's `PUT` method.

This completely replaces the selected destination path with the source data.

```text
PUT /destination.json
```

Replace mode includes an additional confirmation before the write is performed because existing destination data can be overwritten. 
## Example Use Cases

### Database Migration

Move data from an old Firebase project to a new Firebase project.

```text
Old Firebase Project
        │
        ▼
Firebase Duplicator
        │
        ▼
New Firebase Project
```

### Database Consolidation

Combine data from several Firebase projects into a single project by using different destination paths.

```text
Project A ──► legacy/projectA
Project B ──► legacy/projectB
Project C ──► legacy/projectC
```

The application supports destination paths specifically for this type of consolidation workflow.

### Database Backup

Preview the source and download its contents as JSON before performing a migration.

### Project Decommissioning

Copy important Realtime Database data to another project before shutting down or replacing the original project.

## Requirements

This project is intentionally lightweight.

You only need:

- A modern web browser
- Access to the source Firebase Realtime Database
- Access to the destination Firebase Realtime Database
- Appropriate Firebase Realtime Database permissions

The application itself does not require PHP, Node.js, MySQL, or a backend server.

## Running Locally

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/firebase-realtime-database-duplicator.git
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

You can also host the project using GitHub Pages or another static web hosting service.

## Firebase REST API

The application builds Firebase Realtime Database REST endpoints using the database URL, optional path, and optional authentication token.

For example:

```text
https://your-project-default-rtdb.firebaseio.com/users.json
```

The application uses:

```text
GET   → Read source data
PATCH → Merge data into destination
PUT   → Replace destination data
```

These operations are implemented directly in the browser using JavaScript `fetch()`. 
## Security Considerations

**Use this tool carefully with production databases.**

The application sends requests directly from the user's browser to Firebase. Database permissions are therefore controlled by the Firebase Realtime Database Rules and any authentication credentials supplied by the user.

### Important

- Never publish database secrets or authentication tokens in the source code.
- Do not commit credentials to GitHub.
- Only provide authentication credentials to trusted users.
- Verify the destination database before using **Replace** mode.
- Always create a backup before performing a destructive migration.
- Use Firebase security rules appropriate for your environment.

The application masks authentication values when displaying generated endpoints in the interface and requires confirmation before Replace mode writes over the destination. 
## Limitations

This project is intended for typical project-sized Firebase Realtime Database operations.

For very large databases, Firebase's dedicated export/import tools may be more appropriate than transferring the complete dataset through a browser.

The application itself also notes this limitation in its interface.

## Project Structure

```text
firebase-realtime-database-duplicator/
│
├── index.html
└── README.md
```

The current application is implemented as a single standalone HTML file containing the interface, styling, and JavaScript functionality.

## Author

**Alcquimarc**

Facebook:  
https://facebook.com/alcquimarc

## License

This project can be released under the **MIT License** if you want others to freely use, modify, and redistribute it.

A `LICENSE` file should be added to the repository when publishing the project.

## Disclaimer

This tool is provided for legitimate database administration, migration, backup, testing, and development purposes.

The user is responsible for ensuring that they have authorization to access, copy, migrate, or modify the Firebase Realtime Database instances used with this application.

Always verify Firebase security rules and destination paths before performing a migration.
