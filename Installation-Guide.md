# osTicket Installation Guide

## What Is osTicket And Why Use It?

osTicket is a free open source helpdesk ticketing system that allows 
individuals and small to medium businesses to manage support requests 
professionally. It includes the ability to create and assign tickets, 
set priority levels, organize departments, define SLA plans, and track 
every issue from creation to resolution.

There are two main reasons someone would use osTicket:

1. **Practice** — To gain hands on experience with a real ticketing 
   system and understand how helpdesk workflows operate behind the 
   scenes, preparing for a helpdesk or IT support role
2. **Production use** — To give a small business a proper ticketing 
   system without the cost of enterprise solutions like ServiceNow or 
   Jira Service Management

---

## Prerequisites

Before starting you will need:
- A Windows machine
- **XAMPP** — provides the local web server and database environment
- **osTicket v1.18.3** — downloaded from osTicket's official GitHub
- A browser to access the web installer and admin panel

---

## Step 1 — Install XAMPP

Download XAMPP from https://www.apachefriends.org and run the installer.

During installation ensure these components are selected:
- **Apache** — the web server that hosts osTicket locally
- **MySQL** — the database that stores all tickets, users, and settings
- **PHP** — the programming language osTicket is built on
- **phpMyAdmin** — a visual interface for managing the MySQL database

**Why XAMPP?**
osTicket is a web application — it needs a web server and database to 
run. XAMPP bundles everything needed into one simple installation, 
creating a complete local server environment on your machine. Think of 
it as the infrastructure that osTicket lives on, similar to how Active 
Directory needs Windows Server to run.

Once installed open the XAMPP Control Panel and click **Start** next 
to both **Apache** and **MySQL.** Both should turn green confirming 
they are running.

---

## Step 2 — Download osTicket

Download osTicket v1.18.3 directly from the official GitHub releases page:

👉 https://github.com/osTicket/osTicket/releases

Download the file called `osTicket-v1.18.3.zip`

**Why download from GitHub directly?**
The official osTicket website routes you through a customization page 
for plugins and language packs. Downloading directly from GitHub gives 
you the clean base installation without any unnecessary additions.

---

## Step 3 — Configure The Files

**Step 3a — Create the osTicket folder in XAMPP**

Navigate to: C:\xampp\htdocs\

Create a new folder called `osticket` inside htdocs.

**Why htdocs?**
This is Apache's web root folder — anything placed here is served 
as a website. By placing osTicket here it becomes accessible at 
`http://localhost/osticket` in your browser.

**Step 3b — Extract and move the files**

Extract the downloaded ZIP file to your Desktop. Inside you will 
find two folders:

| Folder | Purpose |
|---|---|
| `upload` | The actual osTicket application — this is what we need |
| `scripts` | Command line maintenance tools — not needed for our setup |

Copy everything **inside** the `upload` folder and paste it into 
`C:\xampp\htdocs\osticket\`

**Step 3c — Rename the config file**

Navigate to: C:\xampp\htdocs\osticket\include\

Rename `ost-sampleconfig.php` to `ost-config.php`

**Why rename it?**
osTicket looks specifically for `ost-config.php` to read its 
configuration. The sample file is a template — renaming it activates 
it as the live configuration file.

---

## Step 4 — Create The Database

Open phpMyAdmin by going to: http://localhost/phpmyadmin

Click **New** in the left panel and create a database with these settings:

| Setting | Value |
|---|---|
| **Database name** | `osticket` |
| **Collation** | `utf8_general_ci` |

Click **Create.**

**Why do we need a database?**
osTicket stores everything — tickets, users, agents, settings, and 
history — in a MySQL database. Without it the application has nowhere 
to save any data. The database name `osticket` is what we'll reference 
during the web installer so osTicket knows exactly where to store its data.

A database provides data persistence, without it all application data exists 
only in memory and is lost the moment the server shuts down. By storing data in
MySQL, osTicket ensures everything persists between sessions and is immediately
available when the server restarts.

| Without Database | With Database |
|---|---|
| Data lives in RAM | Data written to disk |
| Lost on shutdown | Survives shutdown |
| No persistence | Full persistence |

**Why utf8_general_ci collation?**
This ensures the database properly handles international characters 
and is case insensitive when comparing text — the standard for web 
applications.

---

## Step 5 — Run The Web Installer

Navigate to: http://localhost/osticket/setup/

**Prerequisites Check**
The installer will check your server configuration. Required items 
must be green. Optional items showing red X are fine to ignore — 
they are performance and email enhancements not needed for our lab.

Required items that must be green:
- PHP v8.2 or greater ✅
- MySQLi extension ✅

**Installation Form**
Fill out the following:

*System Settings:*
| Field | Value |
|---|---|
| Helpdesk URL | `http://localhost/osticket/` |
| Helpdesk Name | Your helpdesk name |
| Default Email | `helpdesk@homelab.local` |

*Admin User:*
| Field | Value |
|---|---|
| First Name | Your first name |
| Last Name | Your last name |
| Username | Create a username |
| Password | A strong password |

*Database Settings:*
| Field | Value |
|---|---|
| MySQL Hostname | `localhost` |
| MySQL Database | `osticket` |
| MySQL Username | `root` |
| MySQL Password | Your MySQL root password |

Click **Install Now** and wait for the success confirmation.

---

## Step 6 — Post Installation Cleanup

After successful installation two cleanup steps are required:

**Step 6a — Delete the setup folder**

Navigate to `C:\xampp\htdocs\osticket\` and delete the `setup` folder entirely.

**Why this is a security requirement:**
Leaving the setup folder accessible after installation means anyone 
who knows the URL could potentially reinstall osTicket and wipe your 
entire configuration and ticket history. Deleting it removes this 
vulnerability completely.

**Step 6b — Reset config file permissions**

Open PowerShell and run:
```powershell
icacls "C:\xampp\htdocs\osticket\include\ost-config.php" /reset
```

**Why this matters:**
During installation osTicket needs write access to the config file 
to save your settings. After installation that write access should 
be removed so the file cannot be modified — protecting your 
configuration from unauthorized changes.

---

## Accessing osTicket

**Staff Control Panel — Agent and Admin login:** http://localhost/osticket/scp

**Customer Portal — Where users submit tickets:** http://localhost/osticket/

**Default Admin Credentials:**
- Username: whatever you set during installation
- Password: whatever you set during installation

---

## Starting and Stopping osTicket

osTicket runs through XAMPP. Every time you want to use it:

**To start:**
1. Open XAMPP Control Panel
2. Click Start next to Apache
3. Click Start next to MySQL
4. Navigate to `http://localhost/osticket/scp`

**To stop:**
1. Click Stop next to MySQL
2. Click Stop next to Apache
3. Close XAMPP Control Panel

Always stop services properly to prevent database corruption.

And that right there is the installation process for osTicket!
