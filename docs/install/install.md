# Installation
**resop** can be used for any Linux cluster, but it is tailored for High Performance Computing (HPC) clusters.
## Pre-requisites
#### Host
> Windows and MacOS not tested, likely compatible but no installation instructions are provided

- Linux
- NodeJS >= v12.20
- CMake
- gcc >= 9.20 (set as default compiler recommended)
- OpenSSL

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
- Install [libssh2](https://github.com/libssh2/libssh2/blob/master/docs/INSTALL_CMAKE.md)
```bash
git clone https://github.com/libssh2/libssh2.git
cd libssh2
mkdir build && cd build
cmake ..
make
make install
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
