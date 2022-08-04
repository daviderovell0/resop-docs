# Implement custom commands or operations
If the command or operation that you are looking for is not already in resop, you can implement your own one. <br>
The API is structured in a way to ease the implementation of new endpoints as much as possible: 

- Commands are implemented with a few lines of `YAML`
- Operations are implemented on single `JavaScript` file. 

This page is a guide to add a new endpoint. 

> Help build a library of useful functional by adding your endpoints to the official repo!

## Operators
Commands and operations are grouped into "operators". Operators serve uniquely as logical categories for better organisation.

In practice, operators are just folders located in `src/operators/`.

New commands or operations can be added to existing operators, or a new operator can be created. The content of the operator folder
needs to follow a precise structure:
```
operator/
├── commands.yml
└── operations/
    ├── operation1.js
    └── operation2.js

```
At runtime, the API will follow this structure to import and build new endpoints automatically.

Multiple commands are defined in the commands.yml file, while each operation is defined using separate JS file. For example, a command named
`command1` inside commands.yml in the above structure will generate the following endpoint:
```
https://<API>/operator/cmd/command1
```
while the 2 operations will generate:
```
https://<API>/operator/opn/operation1
https://<API>/operator/opn/operation2
```

> The **template** operator can be used to build new commands and operations!

## Command

Commands are defined in a YAML file called `commands.yml`, in the corresponding operator folder. Multiple commands can be defined in the same file.

To add a new command, simply edit the [commands.yml]() file and add a new entry, following the template below:

```yaml
- command: test # the command to run
  options: # the command options
    option1:
      flag: -o1 # the option flag
      description: option 1 description # an optional description
    option2:
      flag: -o2
      description: option 2 description

  # form: static/slurm_run_form.html # extra feature not included
  description: This is a description # an optional description
  # enabled: true # not currently used, but can be used to disable certain commands temporarily
```
The *input* field does not need to be specified: it is automatically added by the API for additional arguments or stdin for each command.

Implicitly, the command must correspond to an existing command or script in the remote cluster.

Non-declared-options can be used to filter out unwanted options and flags, allowing admins to put additional control on endpoints.

## Operations

Operations are defined in a JavaScript file in the corresponding operator directory. The file name **must** correspond to the name of the operation. I.e. `myOperation.js -> /{operator}/opn/myOperation`.

To create or edit an operation, use an instance of the *Operation* class. This class provides methods to define your operation, the template example is reported below:

```js
import Operation from '../../../backend/Operation';
import * as otherOPN from '../../operator/operations/otherOPN';

/**
 * @summary Description
 */

// (mustdo) Instatiate a new Operation:
const opn = new Operation();

opn.setDescription('Creates a user in the api database');

// options in the POST request body, to be used in the Operation logic
opn.defineOptions({
  option1: '-> description',
  option2: '-> description',
});

// Create an execution function that will constitute the body of the operation.
// This fucntion will be executed on a POST request to this operation corresponding
// endpoint.
// Can contain any arbitrary logic, be sync or async
function exec() {
  // an Operation can run a command in the remote cluster and read its stdout.
  // The command is executed on behalf of the API user (=cluser user) running it.
  // Error handling is NOT need on commands, it is already done by the Operation.
  // Commands can be of 2 types:

  // standard strings
  const stdout1 = opn.runCommand('my_command --flag option input');

  // use a command defined in this operator's commands, as it would be in a POST request
  const stdout2 = opn.runCommandDefined({
    command: 'command_name',
    option1: 'option_argument',
    input: 'stdin for command name',
  });

  // add log information to the HTTP response and API log
  opn.addLog('commands completed...');

  // any JS code can be used, including exeternal modules
  let out = stdout2;
  out = `${out}, ${stdout1}`;

  // Error handling via .error() method. The Opeation will be interruped at the first .error
  // found
  if (out === null) {
    opn.error('no output from commands!');
  }

  /**
   * other operation can be imported from the correspoding file (see example import at
   * the top of the file). Then it can be used as a command, error handling is also taken care
   * by resop
   */
  const passwordHash = opn.runOperation(otherOPN, {
    field: opn.options.option1,
    option: 'true',
  });

  opn.addLog(passwordHash);

  // the result of the operation
  return out;
}

// (must do), set the execution function
opn.set(exec);

// (mustdo) export the Operation instance
export default opn;


```
Follow the comments for an explaination of each step.

### A few notes
- Embedded operations are supported. 
- No error handling is needed in the operation execution function definition: resop does that automatically
- use `addLog()` to "track progress" or log important information. Log will be displayed as a field of the API response.
- use `error()` to exit the operation upon some chosen conditions. A formatted response will be automatically generated by the API
- for now, `await`s are **compulsory** on all Operation methods. The execution function must also be `async`'ed.   

Once defined, the API will automatically create the corresponding endpoint. See the [overview](/resop-docs/use/overview) for information on the endpoint layout.


