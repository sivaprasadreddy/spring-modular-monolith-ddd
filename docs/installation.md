# Installation Guide

## SDKMAN
Install JDK, Maven, Gradle, etc using [SDKMAN](https://sdkman.io/)

```shell
$ curl -s "https://get.sdkman.io" | bash
$ source "$HOME/.sdkman/bin/sdkman-init.sh"
$ sdk install java 25-tem
$ sdk install maven
$ sdk install gradle
```

## Taskfile
Task is a task runner that we can use to run any arbitrary commands in an easier way.

```shell
$ brew install go-task
(or)
$ go install github.com/go-task/task/v3/cmd/task@latest
```

On Linux you can install `task` using your package manager or via `go install`:

```shell
# Debian/Ubuntu (using apt via the official binary repo):
sudo sh -c "wget -qO - https://taskfile.dev/install.sh | bash"

# or using snap (if available):
sudo snap install task --classic

# or build from source with Go:
go install github.com/go-task/task/v3/cmd/task@latest
```

On Windows you can use Chocolatey, Scoop, or `go install`:

```powershell
# Chocolatey
choco install gotask -y

# Scoop
scoop install task

# or build from source with Go (requires Go installed and in PATH):
go install github.com/go-task/task/v3/cmd/task@latest
```

## Kind Cluster
* [Install kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Install kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
* [Kubernetes Troubleshooting](k8s-troubleshooting.md)

```shell
$ brew install kubectl
$ brew install kind
```

Create a KinD cluster.

```shell
# Create KinD cluster
$ task kind_create

# Destroy KinD cluster
$ task kind_destroy
```
