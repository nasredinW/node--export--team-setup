# Node.js Team Setup Guide

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Node.js](https://img.shields.io/badge/Node.js-LTS-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Platforms](https://img.shields.io/badge/Platforms-Linux%20%7C%20Windows-blue)](#installation)

A cross-platform guide for installing and configuring **Node.js** and **npm** on Linux and Windows, so every developer on the team works in a consistent, reproducible environment.

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
  - [Linux](#linux)
  - [Windows](#windows)
- [Verify Installation](#verify-installation)
- [Quick Start: Your First Project](#quick-start-your-first-project)
- [Working with an Existing Repository](#working-with-an-existing-repository)
- [Useful npm Commands](#useful-npm-commands)
- [Troubleshooting](#troubleshooting)
- [Team Development Standards](#team-development-standards)
- [Official Resources](#official-resources)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**Node.js** is a JavaScript runtime that lets you run JavaScript outside the browser.
**npm** (Node Package Manager) ships with Node.js and is used to install, manage, and share project dependencies.

This guide covers:

- Installing Node.js on Linux and Windows
- Managing multiple Node.js versions
- Verifying your installation
- Creating and running a Node.js project
- Installing dependencies for existing projects
- Following consistent team development practices

## Prerequisites

Before you begin, make sure you have:

- An active internet connection
- Administrator privileges (Windows) or `sudo` access (Linux)
- A terminal application:
  - **Linux:** Terminal
  - **Windows:** PowerShell or Windows Terminal
- [Git](https://git-scm.com/doc) (recommended)

---

## Installation

### Linux

| Method | Best for |
| --- | --- |
| [NVM](#option-1-nvm-recommended) | Development environments, multiple Node.js versions (**recommended**) |
| [APT](#option-2-apt) | Quick setup on Ubuntu/Debian-based distributions |

#### Option 1: NVM (Recommended)

[NVM](https://github.com/nvm-sh/nvm) (Node Version Manager) lets you install and switch between Node.js versions without administrator privileges.

1. **Install NVM** (see the [official instructions](https://github.com/nvm-sh/nvm) for the latest version):

   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
   ```

   Then restart your terminal or reload your shell configuration:

   ```bash
   # Bash
   source ~/.bashrc

   # Zsh
   source ~/.zshrc
   ```

2. **Verify NVM:**

   ```bash
   nvm --version
   ```

3. **Install the latest LTS release:**

   ```bash
   nvm install --lts
   ```

4. **Activate it:**

   ```bash
   nvm use --lts
   ```

5. **Set it as the default:**

   ```bash
   nvm alias default 'lts/*'
   ```

6. **Verify Node.js and npm:**

   ```bash
   node --version
   npm --version
   ```

#### Option 2: APT

Suitable for Ubuntu and Debian-based distributions.

```bash
sudo apt update
sudo apt install -y nodejs npm

node --version
npm --version
```

> **Note:** APT repositories may ship an older Node.js version. If your project requires a specific or recent LTS release, use NVM instead.

---

### Windows

| Method | Best for |
| --- | --- |
| [Official Installer](#option-1-official-installer-recommended-for-beginners) | Beginners (**recommended**) |
| [Winget](#option-2-winget) | Command-line installation |
| [NVM for Windows](#option-3-nvm-for-windows) | Switching between multiple Node.js versions |

#### Option 1: Official Installer (Recommended for Beginners)

1. Visit [nodejs.org](https://nodejs.org/) and download the **LTS** installer (`.msi`) for your system architecture.
2. Run the installer and:
   - Accept the license agreement
   - Choose an installation directory
   - Keep the default options
   - Ensure Node.js is added to the system `PATH`
3. Open PowerShell or Command Prompt and verify:

   ```powershell
   node --version
   npm --version
   ```

#### Option 2: Winget

Using the Windows Package Manager, run in PowerShell:

```powershell
winget install OpenJS.NodeJS.LTS
```

Restart your terminal, then verify:

```powershell
node --version
npm --version
```

#### Option 3: NVM for Windows

> **Important:** [NVM for Windows](https://github.com/coreybutler/nvm-windows) is a separate project from NVM for Linux/macOS, and its commands differ slightly.

1. Download the latest `nvm-setup.exe` from the [releases page](https://github.com/coreybutler/nvm-windows).
2. Run the installer with the default settings, then open a **new** PowerShell window.
3. Verify NVM:

   ```powershell
   nvm version
   ```

4. Install and activate the LTS release:

   ```powershell
   nvm install lts
   nvm use lts
   ```

5. Verify Node.js and npm:

   ```powershell
   node --version
   npm --version
   ```

---

## Verify Installation

On any operating system, run:

```bash
node --version
npm --version
```

Example output (versions will vary with your installed release):

```text
v24.x.x
11.x.x
```

To check where Node.js is installed:

```bash
# Linux
which node
which npm
```

```powershell
# Windows PowerShell
where.exe node
where.exe npm
```

---

## Quick Start: Your First Project

1. **Create a project directory:**

   ```bash
   mkdir nodejs-test
   cd nodejs-test
   ```

2. **Initialize an npm project** (creates a `package.json`):

   ```bash
   npm init -y
   ```

3. **Create `index.js`:**

   ```javascript
   console.log("Node.js is working correctly!");
   ```

4. **Run it:**

   ```bash
   node index.js
   ```

   Expected output:

   ```text
   Node.js is working correctly!
   ```

Your Node.js environment is ready. 🎉

---

## Working with an Existing Repository

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   ```

2. **Navigate to the project directory:**

   ```bash
   cd <project-directory>
   ```

3. **Install dependencies:**

   ```bash
   npm ci        # if a package-lock.json exists (recommended)
   # or
   npm install
   ```

   | Command | Behavior |
   | --- | --- |
   | `npm ci` | Installs exactly what the lockfile specifies. Recommended for CI and reproducible team setups. |
   | `npm install` | Installs dependencies and may update the lockfile. |

4. **Run the project.** Check the `scripts` section of `package.json`, for example:

   ```json
   {
     "scripts": {
       "start": "node index.js",
       "dev": "node --watch index.js"
     }
   }
   ```

   ```bash
   npm start      # run the application
   npm run dev    # start the development environment
   ```

   Available commands depend on each project's configuration.

---

## Useful npm Commands

| Command | Description |
| --- | --- |
| `npm init -y` | Initialize a new project |
| `npm install` | Install project dependencies |
| `npm ci` | Clean install from the lockfile |
| `npm install <package>` | Install a package |
| `npm install -D <package>` | Install a development dependency |
| `npm uninstall <package>` | Remove a package |
| `npm update` | Update dependencies within allowed version ranges |
| `npm list` | Display installed packages |
| `npm run` | List available scripts |
| `npm start` | Execute the `start` script |
| `npm run dev` | Execute the `dev` script |

---

## Troubleshooting

<details>
<summary><strong>1. <code>node</code> command not found</strong></summary>

**Linux (NVM):** reload your shell configuration and confirm NVM is available.

```bash
source ~/.bashrc    # or: source ~/.zshrc
nvm --version
```

**Windows:**

- Restart PowerShell or Command Prompt
- Verify Node.js is included in the system `PATH`
- Check the installation directory
- Restart Windows if necessary

</details>

<details>
<summary><strong>2. npm permission errors on Linux</strong></summary>

If you installed Node.js with NVM, **do not use `sudo` with npm commands**. NVM installs Node.js and npm in your user directory. Confirm your installation paths:

```bash
which node
which npm
```

</details>

<details>
<summary><strong>3. Managing multiple Node.js versions</strong></summary>

**Linux/macOS (NVM):**

```bash
nvm list                 # list installed versions
nvm install 22           # install another version
nvm use 22               # switch versions
nvm alias default 22     # set the default
```

**Windows (NVM for Windows):**

```powershell
nvm list
nvm install 22
nvm use 22
```

</details>

<details>
<summary><strong>4. Port already in use</strong></summary>

Identify the process occupying the port (replace `3000` with your application's port).

```bash
# Linux
sudo lsof -i :3000
```

```powershell
# Windows PowerShell
netstat -ano | findstr :3000
```

</details>

---

## Team Development Standards

To keep environments consistent across the team:

1. **Use the same Node.js major version.** All team members should run the same major release.
2. **Prefer LTS releases** for both development and production.
3. **Commit dependency lockfiles.** Always commit `package.json` and `package-lock.json`.
4. **Use `npm ci`** for fresh installs and CI/CD pipelines.
5. **Pin the Node.js version** by adding an `.nvmrc` file to the project root with the agreed version, for example:

   ```text
   24
   ```

   Linux/macOS (NVM):

   ```bash
   nvm install
   nvm use
   ```

   Windows (NVM for Windows):

   ```powershell
   nvm install 24
   nvm use 24
   ```

6. **Document project scripts.** Keep clear scripts for development, testing, and production in `package.json`.

---

## Official Resources

| Resource | Link |
| --- | --- |
| Node.js Website | https://nodejs.org/ |
| Node.js Documentation | https://nodejs.org/docs/latest/api/ |
| npm Documentation | https://docs.npmjs.com/ |
| NVM (Linux/macOS) | https://github.com/nvm-sh/nvm |
| NVM for Windows | https://github.com/coreybutler/nvm-windows |
| Git Documentation | https://git-scm.com/doc |

---

## Contributing

Contributions and suggestions are welcome! To improve this guide:

1. Fork this repository
2. Create a new branch
3. Make your changes
4. Submit a Pull Request

Please keep installation instructions in line with the official Node.js documentation.

## License

This project is licensed under the [MIT License](LICENSE).

---

<p align="center">
  Maintained for developers who want a simple, consistent, and reliable Node.js environment across Linux and Windows.
</p>
