Node.js Team Setup Guide

A complete cross-platform installation and configuration guide for Node.js and npm on Linux and Windows.

This repository helps developers set up a consistent Node.js development environment quickly and correctly.

Table of Contents
Overview
Prerequisites
Linux Installation
Option 1: Install using NVM (Recommended)
Option 2: Install using APT
Windows Installation
Option 1: Official Installer
Option 2: Install using Winget
Option 3: Install using NVM for Windows
Verify Installation
Create Your First Node.js Project
Install Project Dependencies
Useful npm Commands
Troubleshooting
Team Development Standards
Official Resources
Overview

Node.js is a JavaScript runtime that allows developers to execute JavaScript outside the browser.

npm (Node Package Manager) is the default package manager distributed with Node.js. It helps developers install, manage, and share project dependencies.

This guide covers:

Installing Node.js on Linux distributions.
Installing Node.js on Windows.
Managing Node.js versions.
Verifying the installation.
Creating and running a Node.js project.
Installing project dependencies.
Following consistent development practices.
Prerequisites

Before starting, make sure you have:

An active Internet connection.
Administrator privileges on Windows or sudo access on Linux.
A terminal application:
Linux: Terminal.
Windows: PowerShell or Windows Terminal.
Git (recommended for development).
Linux Installation

There are two installation methods available.

Option 1: Install using NVM (Recommended)

NVM (Node Version Manager) allows developers to install and manage multiple Node.js versions without requiring administrator privileges for normal version management.

This is the recommended method for development environments.

Step 1: Install NVM

Follow the official installation instructions:

https://github.com/nvm-sh/nvm

Alternatively, use the official installation script:

bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash


After installation, restart your terminal or load your shell configuration.

For Bash:

bash
source ~/.bashrc


For Zsh:

bash
source ~/.zshrc

Step 2: Verify NVM
bash
nvm --version

Step 3: Install the latest Node.js LTS version
bash
nvm install --lts

Step 4: Activate the LTS version
bash
nvm use --lts

Step 5: Set the default Node.js version
bash
nvm alias default 'lts/*'

Step 6: Verify Node.js and npm
bash
node --version
npm --version


Installation completed!

Option 2: Install using APT

This method is suitable for Ubuntu and Debian-based distributions.

Step 1: Update package lists

bash
sudo apt update


Step 2: Install Node.js and npm

bash
sudo apt install -y nodejs npm


Step 3: Verify installation

bash
node --version
npm --version

Note: APT repositories may contain an older Node.js version. For development projects requiring a specific or recent LTS version, prefer NVM.
Windows Installation

There are three installation methods available.

Option 1: Official Installer (Recommended for Beginners)

This is the easiest way to install Node.js on Windows.

Step 1: Download Node.js

Visit the official website:

https://nodejs.org/

Download the LTS (Long-Term Support) version for Windows.

Choose the appropriate installer for your system architecture.

Step 2: Install Node.js
Open the downloaded .msi file.
Accept the license agreement.
Choose the installation directory.
Keep the default installation options.
Ensure Node.js is added to the system PATH.
Complete the installation.
Step 3: Verify installation

Open PowerShell or Command Prompt:

powershell
node --version
npm --version


You should see the installed versions.

Option 2: Install using Winget

Windows Package Manager (winget) allows you to install Node.js directly from the terminal.

Open PowerShell and execute:

powershell
winget install OpenJS.NodeJS.LTS


After installation, restart your terminal.

Verify:

powershell
node --version
npm --version

Option 3: Install using NVM for Windows

If you need to switch between different Node.js versions, you can use NVM for Windows.

Important: NVM for Windows is a separate project from the Linux/macOS version of NVM.

Step 1: Download NVM for Windows

Visit:

https://github.com/coreybutler/nvm-windows

Download the latest nvm-setup.exe release.

Step 2: Install NVM
Run the installer.
Follow the installation wizard.
Keep the default settings unless your environment requires otherwise.
Open a new PowerShell terminal.
Step 3: Verify NVM
powershell
nvm version

Step 4: Install Node.js LTS
powershell
nvm install lts

Step 5: Activate Node.js
powershell
nvm use lts

Step 6: Verify installation
powershell
node --version
npm --version


Installation completed!

Verify Installation

Regardless of your operating system, execute the following commands:

bash
node --version
npm --version


Example output (illustrative):

text
v24.x.x
11.x.x


The exact versions depend on the installed Node.js release.

You can also check where Node.js is installed.

Linux:

bash
which node
which npm


Windows PowerShell:

powershell
where.exe node
where.exe npm

Create Your First Node.js Project

Once Node.js and npm are installed, follow these steps to create a test project.

Step 1: Create a project directory
bash
mkdir nodejs-test
cd nodejs-test

Step 2: Initialize an npm project
bash
npm init -y


This creates a package.json file containing the project configuration.

Step 3: Create an index.js file

Create a file named index.js:

javascript
console.log("Node.js is working correctly!");

Step 4: Execute the application
bash
node index.js


Expected output:

text
Node.js is working correctly!


Congratulations! Your Node.js environment is ready.

Install Project Dependencies

When working with an existing Git repository, follow these steps.

Step 1: Clone the repository
bash
git clone <repository-url>

Step 2: Navigate to the project directory
bash
cd <project-directory>

Step 3: Install dependencies

If the project contains a package-lock.json file, use:

bash
npm ci


Alternatively:

bash
npm install


Difference:

npm ci: Installs dependencies based on the lockfile. Recommended for CI environments and reproducible team setups.
npm install: Installs dependencies and may update the lockfile.
Step 4: Run the project

Check the scripts section in package.json.

For example:

json
{
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js"
  }
}


Run the application:

bash
npm start


Or start the development environment:

bash
npm run dev


The available commands depend on the project configuration.

Useful npm Commands
Command	Description
npm init -y	Initialize a new project
npm install	Install project dependencies
npm ci	Clean installation from lockfile
npm install <package>	Install a package
npm install -D <package>	Install a development dependency
npm uninstall <package>	Remove a package
npm update	Update dependencies within allowed version ranges
npm list	Display installed packages
npm run	List available scripts
npm start	Execute the start script
npm run dev	Execute the development script
Troubleshooting
1. Node.js command not found

Linux:

If you installed Node.js using NVM, reload your shell configuration:

bash
source ~/.bashrc


For Zsh:

bash
source ~/.zshrc


Check whether NVM is available:

bash
nvm --version


Windows:

Restart PowerShell or Command Prompt.
Verify that Node.js is included in the system PATH.
Check the installation directory.
Restart Windows if necessary.
2. npm permission errors on Linux

If you installed Node.js using NVM, avoid using sudo with npm commands.

NVM installs Node.js and npm in your user directory.

Check your installation:

bash
which node
which npm

3. Multiple Node.js versions

If you use NVM, you can list installed versions:

bash
nvm list


Install another version:

bash
nvm install 22


Switch versions:

bash
nvm use 22


Set the default version:

bash
nvm alias default 22


On Windows with NVM for Windows, the corresponding commands are:

powershell
nvm list
nvm install 22
nvm use 22

4. Port already in use

If your Node.js application cannot start because a port is already occupied, identify the process using that port.

Linux:

bash
sudo lsof -i :3000


Windows PowerShell:

powershell
netstat -ano | findstr :3000


Replace 3000 with your application's port.

Team Development Standards

To ensure consistency across the development team, follow these recommendations.

1. Use the same Node.js version

All team members should use the same Node.js major version.

2. Use LTS releases

Prefer Node.js LTS releases for development and production environments.

3. Commit dependency lockfiles

Always commit these files to Git:

package.json
package-lock.json

This helps ensure consistent dependency installation.

4. Use npm ci

For fresh installations and CI/CD pipelines, prefer:

bash
npm ci

5. Define the Node.js version

Add an .nvmrc file to the project root containing the agreed version.

For example:

text
24


Linux/macOS developers using NVM can then run:

bash
nvm install
nvm use


NVM for Windows users can use:

powershell
nvm install 24
nvm use 24

6. Document project scripts

Make sure the project's package.json contains clear scripts for development, testing, and production.

Official Resources
Resource	Link
Node.js Official Website	https://nodejs.org/
Node.js Documentation	https://nodejs.org/docs/latest/api/
npm Documentation	https://docs.npmjs.com/
NVM (Linux/macOS)	https://github.com/nvm-sh/nvm
NVM for Windows	https://github.com/coreybutler/nvm-windows
Git Documentation	https://git-scm.com/doc
Contributing

Contributions and suggestions are welcome!

If you find an issue or want to improve this guide:

Fork this repository.
Create a new branch.
Make your changes.
Submit a Pull Request.

Please keep installation instructions up to date with official Node.js documentation.

License

This project is available under the MIT License.

Maintained for developers who want a simple, consistent, and reliable Node.js development environment across Linux and Windows.
