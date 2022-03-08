# Deployment
The API can be hosted anywhere with SSH access to the cluster. Obviously, this includes any local machine, for personal use only. 

## Configuration
Configuration  is made via environment variables taken from the `.env` file in the main repository directory. The API will automatically load the environment variable when starting. 

Rename `.env.example` to `.env` and fill variables with the corresponding values for you setup. An example is reported below:
```shell
### API settings ###

# The API can be set to use HTTPS by setting this
# to 'true'. if the variable is not set or set to
# any other value, the API will use HTTP instead
ENABLE_HTTPS= 'true'

# if ENABLE_HTTPS= 'true' set 
# the variables below to point at the SSL files
SSL_PRIVATE_KEY = '/path/to/your/key'
SSL_CERTIFICATE = 'path/to/your/certificate'

# string used to encrypt JSON Web tokens (JWT) for authentication
# This value should not be disclosed. 
JWT_SECRET = 'random_secretString!'

# Enable the database for logging (@TODO) or custom objects to be stored (@TODO)
# if ENABLE_DB = 'true', an existing MariaDB database needs to be accessible by the API
ENABLE_DB = 'true'
# MariaDB db credentials for database functionalities
# vars below ignored if ENABLE_DB != 'true'
DB = 'myDB'
DB_USER = 'myDBUser'
DB_PWD = 'myDBPassword'
DB_HOST = 'localhost'
DB_PORT = '3306'
DB_LOGGING = false

# Cluster access
# change this with your own cluster settings
CLUSTER_NAME="vagrantcluster"
CLUSTER_ADDRESS="127.0.0.1"
CLUSTER_SSH_PORT="2200"
# location where users private keys are saved, in the server where the API is hosted
# see authentication in docs to know more  
CLUSTER_USERS_SSH_KEY_LOCATION="/tmp"

### Operator settings ###

# custom variables can be defined and used in user-implemented operators.
ANYTHING="thatneedstobesecret"

```
All variables under `API setings` need to be set, unless `ENABLE_HTTPS` or `ENABLE_DB'` are set to *false*. In that case, `SSL`* and `DB`* variables respectively can be left empty.

**resop** will run checks on the configuration file before starting the application, check the logs for additional information.
## Start the API

On production environnements run:

```bash
npm run prod
```
If the configuration is valid, **resop** will attempt to start at port `3000` of the current server: `https://myhost.com:3000`. 

If the API is running correctly, you should see the following message when accessing the server instance:
```JSON
message	"Welcome to resop API. I am ready to work!
```