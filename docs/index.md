# resop (or resop API): REmote Shell Operation API

A RESTful API to manage user, admin, job, (**any**) *shell* operations involving in remote linux cluster. It is specifically targeted to a supercomputer / HPC cluster but can be used for any machine with SSH access. <br>
A thin layer to make classic supercomputer Linux system easily accessible (via HTTP) and encode fully customisable operations into API endpoints.


## Features
- Run remote **Linux commands** through **HTTP** directly to a HPC cluster and get a JSON response.
- Commands and other arbitrary coding can be chained together to create more complex **Operations** that can be submitted through a browser or a bare POST request and get a JSON response.
- *Secure commands and operations*: every endpoint is accessed prior token-based authentication and will run remote operations with the privildeges of the corresponding authenticated user.  
- *Simple customisation*: you can implement custom operations without dealing with API logic. Just create a JS file, create an instance of the Operation class and the API endpoint will be automatically accessible
- *Quick setup*: NodeJS and MariaDB are the only dependencies, one file configuration and the API is ready to be used
- *Run anywhere*: the API uses remote SSH authentication so it can be hosted anywhere that has access to the HPC cluster 
- *Connect 3rd party apps*: as any pther RESTful API, you can easily connect to web or destop apps (portals, IDEs, local CLI apps etc.). Call the API from any programming language -> read the result -> done!

## Use-cases
@todo