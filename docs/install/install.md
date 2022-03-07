# Installation
**resop** can be used for any Linux cluster, but it is tailored for classic High Performance Computing clusters environment 
## Pre-requisites
### For local deployments:

- NodeJS >= v12.20
- MariaDB >= v15, 

### For production environment:

- same as local
- Linux cluster requirements:
    - Cluster users
    - Cluster superuser with passwordless sudo privileges
    - SSH enabled

## Installation

#### Create a database and a user
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
#### Install dependecies
- Clone the repository
- Install NPM dependencies
```bash
cd resop
npm install
```