# taskfiles

[![GitHub Release](https://img.shields.io/github/v/release/standardloop/taskfiles?sort=semver)](https://github.com/standardloop/taskfiles/releases)

---

[https://github.com/standardloop/taskfiles](https://github.com/standardloop/taskfiles)

My collection of re-usable [Taskfiles](https://github.com/go-task/task)

## How to use

### Newest version of task

- https://taskfile.dev/docs/remote-taskfiles

#### Your Taskfile

```yaml
vars:
  STANDARDLOOP_TASKFILES_VERSION: "v0.0.6"

includes:
  color: https://raw.githubusercontent.com/standardloop/taskfiles/refs/tags/{{.STANDARDLOOP_TASKFILES_VERSION}}/color.yml
  rancher: https://raw.githubusercontent.com/standardloop/taskfiles/refs/tags/{{.STANDARDLOOP_TASKFILES_VERSION}}/rancher.yml
  colima: https://raw.githubusercontent.com/standardloop/taskfiles/refs/tags/{{.STANDARDLOOP_TASKFILES_VERSION}}/colima.yml
  c: https://raw.githubusercontent.com/standardloop/taskfiles/refs/tags/{{.STANDARDLOOP_TASKFILES_VERSION}}/c.yml
```

## Color

### Task List

<!-- color.yml TASKS_START -->

```sh
task: Available tasks for this project:
* default
* test      Prints out all the colors avaiable.
```

<!-- color.yml TASKS_END -->

This Taskfile is for sharing variables that allow for settings text color.

![alt text](https://raw.githubusercontent.com/standardloop/taskfiles/refs/heads/main/docs/color.png)

### Using in your Taskfile

When you include this Taskfile you have access to all the color variables.

In general, you will use it like this:

`{{.TEXT_YOUR_CHOICE_HERE}} your message here {{.TEXT_RESET}}`

Example:

```yaml
---
version: "3"

vars:
  STANDARDLOOP_TASKFILES_VERSION: "v0.0.6"

includes:
  color: https://raw.githubusercontent.com/standardloop/taskfiles/refs/tags/{{.STANDARDLOOP_TASKFILES_VERSION}}/color.yml

tasks:
  default:
    silent: true
    cmds:
      - echo "{{.TEXT_YELLOW}}waiting....{{.TEXT_RESET}}"
      - echo "{{.TEXT_GREEN}}DONE{{.TEXT_RESET}}"
```

## Rancher

This Taskfile contains tasks for spinning up [rancher docker engine](https://github.com/rancher-sandbox/rancher-desktop/).

### Task List

<!-- rancher.yml TASKS_START -->

```sh
task: Available tasks for this project:
* start   Start Rancher Docker Engine
* clean   Shutdown Rancher Docker Engine
```

<!-- rancher.yml TASKS_END -->

### Using in your Taskfile

Example:

```yml
---
version: "3"

vars:
  STANDARDLOOP_TASKFILES_VERSION: "v0.0.6"

includes:
  rancher: https://raw.githubusercontent.com/standardloop/taskfiles/refs/tags/{{.STANDARDLOOP_TASKFILES_VERSION}}/rancher.yml

env:
  DOCKER_ENGINE_CPUS: 4
  DOCKER_ENGINE_MEMORY: 8

tasks:
  default:
    silent: true
    cmds:
      - task: rancher:start
        vars:
          FLAGS: "--virtual-machine.number-cpus {{.DOCKER_ENGINE_CPUS}} --virtual-machine.memory-in-gb {{.DOCKER_ENGINE_MEMORY}}"
```

Running:

```sh
$ task
INFO[0000] About to launch /usr/bin/open -a /Applications/Rancher Desktop.app --args --application.startInBackground=true --virtualMachine.memoryInGB 8 --virtualMachine.numberCPUs 4 ...
Waiting for docker to come up...
Waiting for docker to come up...
Waiting for docker to come up...
Waiting for docker to come up...
Docker came up!

$ task clean
Shutting down.
```

## Colima

This Taskfile contains tasks for spinning up [colima docker engine](https://github.com/abiosoft/colima).

### Task List

<!-- colima.yml TASKS_START -->

```sh
task: Available tasks for this project:
* start   Start up colima.
* clean   Delete colima, automatically say yes.
```

<!-- colima.yml TASKS_END -->

### Using in your Taskfile

Example:

```yaml
---
version: "3"

vars:
  STANDARDLOOP_TASKFILES_VERSION: "v0.0.6"

includes:
  colima: https://raw.githubusercontent.com/standardloop/taskfiles/refs/tags/{{.STANDARDLOOP_TASKFILES_VERSION}}/colima.yml

env:
  DOCKER_ENGINE_CPUS: 4
  DOCKER_ENGINE_MEMORY: 8

tasks:
  default:
    silent: true
    cmds:
      - task: colima:start
        vars:
          FLAGS: "--network-address --cpu {{.DOCKER_ENGINE_CPUS}} --memory {{.DOCKER_ENGINE_MEMORY}}"
  clean:
    cmds:
      - task: colima:clean
```

Running:

```sh
$ task
INFO[0000] starting colima
INFO[0000] runtime: docker
INFO[0000] creating and starting ...                     context=vm
INFO[0020] provisioning ...                              context=docker
INFO[0021] starting ...                                  context=docker
INFO[0022] done

$ task clean
are you sure you want to delete colima and all settings? [y/N] INFO[0000] deleting colima
INFO[0000] deleting ...                                  context=docker
INFO[0000] done
```

## C

Reusable tasks to:

- format code
- generate docs
- compile
- compile with address sanitizing
- run test
- download my dylib dependencies

### Task List

<!-- c.yml TASKS_START -->

```sh
task: Available tasks for this project:
* release                           Build the dylib.
* test:build                        Build the test program.
* test:build-sanitize               Build the test with address sanitizer on.
* test:clean
* dependencies:helper-task
* dependencies:helper-get-latest
* dependencies:get-latest
* dependencies:logger
* dependencies:logger:latest
* dependencies:util
* dependencies:util:latest
* dependencies:collections
* dependencies:collections:latest
* dependencies:json:latest
* dependencies:testing
* dependencies:testing:latest
* fmt
* docs
* docs:doxygen                      https://www.doxygen.nl/
* docs:moxygen                      https://0state.com/moxygen
* docs:readme
```

<!-- c.yml TASKS_END -->

## Clearing cache

```sh
$ rm ~/.task/remote/*.yaml
$ rm ~/.task/remote/*.checksum
$ rm ~/.task/remote/*.timestamp
```
