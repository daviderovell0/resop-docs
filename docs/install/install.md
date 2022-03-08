# Installation
**resop** can be used for any Linux cluster, but it is tailored for High Performance Computing (HPC) clusters.
## Pre-requisites
#### Host

- NodeJS >= v12.20

> For future logging functionality only (not in current version): MariaDB >= v15 
#### Remote cluster
- Linux-based OS
- Default shell: bash 
- SSH enabled and accessible by resop API host.

## Installation

#### Install dependecies
- Clone the repository
```bash
git clone https://github.com/daviderovell0/resop.git
```
- Install NPM dependencies
```bash
cd resop
npm install
```

#### Create a database and a user
> For future logging functionality only (not in current version)

Access the MariaDB shell
```bash
sudo mariadb
```
Create an empty MariaSQL database for the app
```sql
CREATE USER admin@localhost IDENTIFIED BY 'admin';
CREATE DATABASE api_db;
GRANT ALL PRIVILEGES on api_db.* TO 'admin'@'localhost';
```
