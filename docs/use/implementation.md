# Implement custom commands or operations
If the command or operation that you are looking for is not already in resop, you can implement your own one. <br>
The API is structured in a way to ease the implementation of new endpoints as much as possible: 

- Commands are implemented with a few lines of `YAML`
- Operations are implemented on single `JavaScript` file. 

This page is a guide to add a new endpoint. 

> It is very encouraged to do so, to help build a library of useful functional by adding your changes to the official repo!

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

## Command YAML structure

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

## Operations