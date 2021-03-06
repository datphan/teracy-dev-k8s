# teracy-dev-k8s

Setting up k8s cluster on teracy-dev (v0.6) with kubespray for a production ready local k8s cluster.
This can be considered a local managed k8s service that we can use it for testing, it should work
the same as any k8s cluster in the cloud.


## How to use

Configure `workspace/teracy-dev-entry/config_default.yaml` with the following similar content:

```yaml
teracy-dev:
  extensions:
    - _id: "entry-0" # this _id is used for overriding by the config_override.yaml file
      path:
        extension: teracy-dev-k8s
      location:
        git: https://github.com/teracyhq-incubator/teracy-dev-k8s.git
        branch: develop
      require_version: ">= 0.1.0-SNAPSHOT"
      enabled: true
```


See this example setup: https://github.com/teracyhq-incubator/teracy-dev-entry-k8s#how-to-use


## Ansible Options

By default, we copy the sample inventory from kubespray into `workspace/inventory` if not exists yet,
so you can configure ansible from the `workspace/inventory` directory.


## Accessing Kubernetes API

You should see the generated artifacts within the `workspace/inventory/artifacts` directory

You can set the `KUBECONFIG` env var for `kubectl` to work:

```
export KUBECONFIG=~/k8s-dev/workspace/inventory/artifacts/admin.conf
```

Or you can append this config into the `~/.kube/config` file:

```
$ cd workspace/inventory/artifacts/
$ cat admin.conf > ~/.kube/config # append the generated admin config to the config file
```

Use it:

```
$ kubectl config use-context admin-cluster.local
$ kubectl cluster-info
```


## Configuration Override

To override default config, you need to create `workspace/teracy-dev-entry/config_override.yaml` to
override the values from `teracy-dev-k8s/config_default.yaml`.

For example:

```yaml
teracy-dev-k8s:
  ansible:
    mode: host
    verbose: vv
  vm_memory: 1600
  vm_cpus: 4
  num_instances: 3
```


## How to develop

Configure `workspace/teracy-dev-entry/config_override.yaml` with the following similar content:

- Configure as follows:

```yaml
teracy-dev:
  extensions:
    - _id: "entry-0" # make sure the right _id matching from the config_default.yaml file
      path:
        lookup: workspace
      location:
        git: git@github.com:hoatle/teracy-dev-k8s.git # your forked repo
```


Enjoy and happy hacking!
