# Implement custom commands or operations

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

  temp_file: true | false # for commands that need a temp file
  form: static/slurm_run_form.html # path to the static form file
  description: This is a description # an optional description
  enabled: true # not currently used, but can be used to disable certain commands temporarily
```
