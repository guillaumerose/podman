% podman-exec(1)

## NAME
podman\-exec - Execute a command in a running container

## SYNOPSIS
**podman exec** *container* [*options*] [*command* [*arg* ...]]

## DESCRIPTION
**podman exec** executes a command in a running container.

## OPTIONS
**--env, -e**

You may specify arbitrary environment variables that are available for the
command to be executed.

**--interactive, -i**

Not supported.  All exec commands are interactive by default.

**--latest, -l**

Instead of providing the container name or ID, use the last created container. If you use methods other than Podman
to run containers such as CRI-O, the last started  container could be from either of those methods.

**--privileged**

Give the process extended Linux capabilities when running the command in container.

**--tty, -t**

Allocate a pseudo-TTY.

**--user, -u**

Sets the username or UID used and optionally the groupname or GID for the specified command.
The following examples are all valid:
--user [user | user:group | uid | uid:gid | user:gid | uid:group ]

**--workdir**, **-w**=""

Working directory inside the container

The default working directory for running binaries within a container is the root directory (/).
The image developer can set a different default with the WORKDIR instruction, which can be overridden
when creating the container.

## Exit Status

The exit code from `podman exec` gives information about why the command within the container failed to run or why it exited.  When `podman exec` exits with a
non-zero code, the exit codes follow the `chroot` standard, see below:

**_125_** if the error is with podman **_itself_**

    $ podman exec --foo ctrID /bin/sh; echo $?
    Error: unknown flag: --foo
    125

**_126_** if the **_contained command_** cannot be invoked

    $ podman exec ctrID /etc; echo $?
    Error: container_linux.go:346: starting container process caused "exec: \"/etc\": permission denied": OCI runtime error
    126

**_127_** if the **_contained command_** cannot be found

    $ podman exec ctrID foo; echo $?
    Error: container_linux.go:346: starting container process caused "exec: \"foo\": executable file not found in $PATH": OCI runtime error
    127

**_Exit code_** of **_contained command_** otherwise

    $ podman exec ctrID /bin/sh -c 'exit 3'
    # 3

## EXAMPLES

$ podman exec -it ctrID ls
$ podman exec -it -w /tmp myCtr pwd
$ podman exec --user root ctrID ls

## SEE ALSO
podman(1), podman-run(1)

## HISTORY
December 2017, Originally compiled by Brent Baude<bbaude@redhat.com>