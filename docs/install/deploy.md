# Deployment
The API can be hosted on the Linux cluster itself or anywhere with SSH access to the cluster. The 2 environments can therefore be separated, the API could be run locally, for example. 



## Configuration
Configuration is needed to for database and remote cluster access. It is done via environment variables taken from the `.env` file in the main repository directory.

- Rename `.env.example` to `.env` and fill variables with the corresponding values for you setup:
  - Database credentials: `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`...
  - Remote access credentials: `CLUSTER_ADDRESS`, `CLUSTER_USERNAME`, `CLUSTER_PASSWORD`...
  - Certificates: ...
  - etc.


## Deploy prod test cluster locally
@TODO

## Start the API
Resop can run in production or development mode.
#### Locally

Run the local server for development purposes with:
```bash
npm run dev:watch
```  
This way, resop can be run "clusterless", meaning that the API will not attempt to connect to a cluster. Some features are disabled when running locally:

- Authentication
- Remote shell commands (both bare command endpoints and commands within an Operation) are not effectively submitted. A success reply will always be returned with the command string generated.

#### Production
On production **environnements** run:

```bash
npm run prod
```
Commands and operations are effetively submitted on the remote cluster 
after a connection attempt has made with the information provided in the JWT
token in the HTTP request. The token is granted after sucessful authentication of a cluster user.