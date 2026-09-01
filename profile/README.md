# go-remoteexec

[![License](https://img.shields.io/badge/license-BSD--3--Clause-blue.svg)](https://github.com/go-remoteexec/transport/blob/main/LICENSE)
[![Pure Go](https://img.shields.io/badge/pure%20Go-CGO%3D0-00ADD8?logo=go&logoColor=white)](https://github.com/go-remoteexec/transport)
[![Go Reference](https://pkg.go.dev/badge/github.com/go-remoteexec/transport.svg)](https://pkg.go.dev/github.com/go-remoteexec/transport)

**Run a command on another machine.**

Every orchestrator needs the same four things underneath: open a connection to a
host, become somebody else, run a command, get the output and the exit status
back. Ansible calls them connection and become plugins; Bolt calls them
transports. They are the same code, and writing it twice means fixing it twice.

So it lives here on its own, and both call it.

## What is here

**[transport](https://github.com/go-remoteexec/transport)** — connections and
privilege escalation, pure Go with `CGO_ENABLED=0`.

- **local** — run on this machine, without a shell where none is needed.
- **ssh** — `golang.org/x/crypto/ssh`, not an `exec.Command` to the `ssh`
  binary: a library reports an authentication failure as an error, where a
  wrapped CLI reports it as text on stderr that has to be parsed and will change.
- **winrm** — WS-Management over HTTP, so Windows hosts are reached the same way
  as any other.
- **sudo / su / doas** — escalation as a wrapper around a connection rather than
  a flag on it, so any transport can carry any of them.

## Who calls it

- [go-ansible](https://github.com/go-ansible) — its connection and become plugins.
- [go-puppet-bolt](https://github.com/go-puppet-bolt) — task and plan
  orchestration over SSH and WinRM.

Part of the **Configuration management** family of the
[pure-Go ecosystem](https://go-desktop.github.io/): no cgo, no shelling out to a
command-line tool in place of a library, built on six 64-bit architectures, 100%
statement coverage as a CI gate, BSD-3-Clause.
