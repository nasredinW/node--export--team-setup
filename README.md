Node.js Installation Guide

This guide explains how to install Node.js and npm on Linux and Windows development machines.

1. Prerequisites

Before installing Node.js, make sure you have:

Administrator or sudo privileges.
An active Internet connection.
A terminal or command prompt.
2. Linux Installation
Ubuntu / Debian

Update your package list:

bash
sudo apt update


Install Node.js and npm:

bash
sudo apt install -y nodejs npm


Check the installed versions:

bash
node --version
npm --version

Recommendation: For development projects, use a Node.js LTS version. The default Ubuntu/Debian repositories may provide an older version.
Recommended: Install using NVM

NVM (Node Version Manager) allows developers to install and switch between multiple Node.js versions.

Install NVM using its official installation instructions:

https://github.com/nvm-sh/nvm

Then install the latest LTS version:

bash
nvm install --lts


Activate it:

bash
nvm use --lts


Set it as the default:

bash
nvm alias default 'lts/*'


Verify:

bash
node --version
npm --version

3. Windows Installation
Option A: Official Installer (Recommended for Beginners)
Visit the official Node.js website: https://nodejs.org/
Download the LTS version for Windows.
Run the downloaded .msi installer.
Follow the installation wizard.
Keep the default installation options.
Ensure the option to add Node.js to PATH is enabled.
Complete the installation.

Open PowerShell or Command Prompt and verify:

powershell
node --version
npm --version

Option B: Install Using winget

Open PowerShell and run:

powershell
winget install OpenJS.NodeJS.LTS


Restart your terminal and verify:

powershell
node --version
npm --version

Option C: Install Using NVM for Windows

For developers who need multiple Node.js versions:

Visit: https://github.com/coreybutler/nvm-windows
Download and install the latest nvm-setup.exe release.
Open a new PowerShell terminal.

Install the latest LTS version:

powershell
nvm install lts


Activate it:

powershell
nvm use lts


Verify:

powershell
node --version
npm --version

4. Verify the Installation

Regardless of your operating system, execute:

bash
node --version
npm --version


You should see version numbers similar to:

text
v24.x.x
11.x.x


The exact versions may differ depending on the current LTS release.

5. Create a Test Node.js Project

Create a project directory:

bash
mkdir nodejs-test
cd nodejs-test


Initialize an npm project:

bash
npm init -y


Create an index.js file:

javascript
console.log("Node.js is working correctly!");


Run the application:

bash
node index.js


Expected output:

text
Node.js is working correctly!

6. Install Project Dependencies

If you are working with an existing team project, navigate to its directory and run:

bash
npm install


This installs the dependencies specified in package.json and follows the lockfile when available.

To start the application, check the available scripts in package.json and run the appropriate command, for example:

bash
npm run dev

7. Common Troubleshooting
Node.js command not found

Linux:

bash
source ~/.bashrc


If using NVM, ensure it is loaded in your shell configuration.

Windows:

Close and reopen PowerShell.
Check that the Node.js installation directory is included in the system PATH.
Restart the terminal after installation.
npm permission errors on Linux

If using NVM, avoid installing global npm packages with sudo. NVM manages Node.js and npm in your user directory.

Multiple Node.js versions

Use NVM to switch versions:

bash
nvm list
nvm use <version>

8. Official Resources
Node.js: https://nodejs.org/
npm documentation: https://docs.npmjs.com/
NVM for Linux/macOS: https://github.com/nvm-sh/nvm
NVM for Windows: https://github.com/coreybutler/nvm-windows
Team Recommendation

For consistency across the team:

Use the same Node.js LTS major version.
Prefer NVM on Linux and NVM for Windows when multiple versions are needed.
Commit package.json and package-lock.json to Git.
Use npm ci for reproducible dependency installation in CI environments.
Document the required Node.js version in the project's README or .nvmrc file.

The team should agree on one Node.js version before starting development.
