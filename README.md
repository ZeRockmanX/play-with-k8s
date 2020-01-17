Play with k8s clusters
------

## Requirements

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* [Virtualbox](https://www.virtualbox.org)
* [Vagrant](https://www.vagrantup.com)
* [Ansible](https://www.ansible.com) (Optional)

**Please note that current version of Vagrant (2.2.6) only supports Virtualbox version up to 6.0.x, please do not install Virtualbox 6.1.**

If you install Ansible on your host machine is not an option, please check *Windows User* section.

## Usage

1. Install requirements from Ansible Galaxy.
```bash
> ansible-galaxy install -r requirements.yml
```

2. Start Vms using Vagrant
```bash
> vagrant up
```

3. Install kubernetes clusters using Ansible playbook.
```bash
> ansible-playbook -i inventory kubernetes.yml
```

4. Connect to k8s cluster using config
```bash
> export KUBECONFIG=kubeconf/k8s-master-01_admin.conf
> kubectl get nodes
```

5. Copy the granted token from generated dashboard_token.log
```bash
cat dashboard_token.log | grep '^token'
```

6. Run proxy for the Dashboard
```bash
> kubectl proxy
```

7. Open the Dashboard UI and login using the token.
[http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)



## Others

### Install Dashboard Only

```bash
> ansible-playbook -i inventory dashboard.yml
```

### Regenerate dashboard token

```bash
> ansible-playbook -i inventory dashboard_token.yml
```


## Windows User

Because Ansible does not support Windows host, we can use one of the nodes of the k8s clusters as the Ansible host.
I've configured Vagrant to provision the kubernetes using the master node as the Ansible host.
So please use the following orders to startup the clusters.

1. Start Vms using Vagrant
```cmd
$ vagrant up
```

2. Provision kuberneters using Vagrant provision.
```cmd
$ vagrant provision --provision-with kubernetes
```

3. Use your k8s clusters.

4. You can also run Ansible playbook after ssh into the master node, the project folder will be automatically mounted to `/vagrant`, and Ansible should already be installed.
```cmd
$ vagrant ssh k8s-master-01
> cd /vagrant
> ansible-playbook -i inventory dashboard_token.yml
```
