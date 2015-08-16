# Transactional Ruby Scripts

Application operations have undo. Database operations have transactions. Why should your scripts be left out? **Transactional Ruby Scripts** allow operators to build scripts for managing their systems that have predictable and recoverable failure modes. No more wondering what state the system has been left in after one failed command.

## Features

## Future Features

* Transactional shell command pairs:

  ```ruby
  TRS.transaction do
    TRS.up do
      system "/etc/init.d/postgresql start"
    end
    TRS.down do
      system "/etc/init.d/postgresql stop"
    end
  end
  ```

* Helpers for commonly-used commands

* Atomic continuations:

  ```
  $: do_somthing.trs
  > mv /tmp/foo /tmp/bar
  ERROR: /tmp/foo does not exist! Rolling back!
  To continue, run:
  > do_something.trs --continue stage_3
  ```

* Nicely-named checkpoints:

  ```ruby
  TRS.checkpoint(:setup_configs, "Copy configuration files into place")
  TRS.copy("/tmp/staging/nginx.conf", "/etc/nginx/nginx.conf")
  ```

  ```
  $: do_something.trs
  PASS CHECKPOINT Copy configuration files into place
  COPY /tmp/staging/nginx.conf -> /etc/nginx/nginx.conf
  ERROR: /tmp/staging/nginx.conf does not exist! Rolling back!
  To continue, run:
  > do_something.trs --continue setup_configs_1
  ```