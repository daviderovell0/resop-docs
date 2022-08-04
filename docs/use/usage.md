# Usage guidance
**resop** uses a modular structure to organise endpoints. Each endpoints correspond either to a 
command or an operation. Commands and operations are both subpaths of an operator:
```bash
/{operator}/cmd/{command}     <- command   
/{operator}/opn/{operation}   <- operation
```
More details about endpoints in the [overview](/resop-docs/use/overview).

Most endpoints are protected and can be accessed only with a token post-authentication. More in the
[auth](/resop-docs/use/auth) section.

A list of all the available endpoints can be found in the [reference](/resop-docs/reference).

## API calls (requests)

Each endpoint can be called with both GET and POST. 

- **GET** requests return informative data about the endpoint: description and available options.
They are internal to the API and will not interact with the remote cluster.
- **POST** requests perform an action and return the its result output with additional information such as possible error details and logs. They often, but not always, interact with the remote cluster.

Key-value options in the request body (i.e. form fields) for both commands and operations allow for specific options. Some option might
be mandatory depending on the implementation of the action. *All fields are sanitized to filter special shell characters*;
this is done to avoid command injection.

A good workflow is to use **GET** to retrieve the available options and then use **POST** with the right options.

Additional GET (only) endpoints in the following format:
```
/{operator}/commands
/{operator}/operations
```
return a list of available commands or operations, respectively. 
## Response object 
In the current version, output and error responses might not be consistent. Below are
possible examples of responses
### Command
#### GET
```
{
    "command": "salloc",
    "description": "salloc  is  used  to allocate a Slurm job allocation",
    "options": {
        "account": {
            "description": "Charge resources used by  this  job  to  specified  account",
            "flag": "--account"
        },
        ...
        "exclusive": {
            "description": "The job allocation can not share nodes with other running jobs",
            "flag": "--exclusive"
        },
        ...
    }
}
```
#### POST
- Execution error
```
{
    "success": false,
    "command": "ldapsearch -b \"dc=xf,dc=priv\" -LLL  -x  \"cn=testproject\"",
    "output": "bash: ldapsearch: command not found\n"
}
```
- InputFormatError
```
{
    "success": false,
    "input": "\"cn=testproject\" &",
    "output": "InputFormatError: character & not allowed, escape it with a backslash"
}
```
- Successful command
```
{
    "success": true,
    "command": "salloc ",
    "output": 0
}
```
### Operation

#### GET
```
{
    "name": "sbatchd",
    "operator": "slurm",
    "description": "sbatch direct: sbatch taking an input files as option",
    "options": {
        "script": "SBATCH submission script (as string)"
    }
}
```
#### POST
- Successful request
```
{
    "success": true,
    "operation": "linux-user:hashMD5",
    "output": "",
    "log": [
        "Hash generation: MD5...",
        "adding escape characters..."
    ]
}
```
- OperationError: an error coming from the operation execution
```
{
    "success": false,
    "operation": "linux-user:hashMD5",
    "output": "OperationError",
    "log": [
        "invalid escapeChar type"
    ]
}
```
- InternalError: something in the code is not right. It could be the operation definition
```
{
    "success": false,
    "operation": "linux-user:createUser",
    "output": "InternalError",
    "log": "message"
}
```
## core/ endpoints
Special "general" endpoints are leaves of the `core/` operator. These endpoints provide direct access
to the main SSH functionalities:

- `core/opn/run`: run any remote command without shell special characters filter (pipe, & etc. allowed)
- `core/opn/getFile`, `core/opn/sendFile`: [scp](https://www.ssh.com/academy/ssh/scp) files from
and to the remote cluster
- `core/opn/getFolder`, `core/opn/sendFolder`: [scp](https://www.ssh.com/academy/ssh/scp) doler from
and to the remote cluster

Other endpoints are specific and can be used (or built) to perform complex shell operations.