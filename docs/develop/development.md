# Development guide

## Pre-requisites

- NodeJS >= v12.20
- MariaDB >= v15

## Configure

See the [deployment page](/resop/install/deploy) for example configurations. 

### Configuration to use the test cluster (vagrantcluster)

**resop** comes with a test cluster, outlined in the [previous section](/resop/develop/vagrantcluster), called vagrantcluster. The cluster can be used to test newly implemented functionalities in a virtual HPC environment, maximizing portability to a real environment.   

Once vagrantcluster is up and running, your `.env` file should look similar to:
```bash
### API settings ###

# disable https for local testing
ENABLE_HTTPS= 'false'

# needed 
JWT_SECRET = 'random_secretString!'

# diasable logging
ENABLE_DB = 'false'

# change this with your own cluster settings
CLUSTER_NAME="devcluster"
CLUSTER_ADDRESS="127.0.0.1"
CLUSTER_SSH_PORT="2200" # check this is the right port to access **fe1** VM
# location where users private keys are saved, in the server where the API is hosted
# see authentication in docs to know more  
CLUSTER_USERS_SSH_KEY_LOCATION="/tmp"

### Operator settings ###

# custom variables can be defined and used in user-implemented operators.
ANYTHING="thatneedstobesecret"
```

## Start the server

Resop can run in production or development mode.

#### Local

Run the local server for development purposes with:
```bash
npm run dev:watch
```
This way, resop can be run "clusterless", meaning that the API will not attempt to connect to a cluster. Some features are disabled when running locally:

- Authentication
- Remote shell commands (both bare command endpoints and commands within an Operation) are not effectively submitted. A success reply will always be returned with the command string generated.

`dev:watch` mode uses [nodemon](https://nodemon.io/) will monitor any changes and rebuild the project anytime a new modification is saved.

#### Production

```bash
npm run prod
```
Commands and operations are effetively submitted on the remote cluster 
after a connection attempt has made with the information provided in the JWT
token in the HTTP request. The token is granted after sucessful authentication of a cluster user.
