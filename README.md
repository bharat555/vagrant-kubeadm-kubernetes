
# Vagrantfile and Scripts to Automate Kubernetes Setup using Kubeadm using CentOS_8_Stream

## Documentation

Current k8s version : 1.22.8-0

## Prerequisites

1. Working Vagrant setup
2. 8 Gig + RAM workstation as the Vms use 3 vCPUS and 4+ GB RAM
3. A Box image with vbguest addons installed

## Create Box with vbguest addons
1. download a base image, sample vagrant file below
```shell
Vagrant.configure("2") do |config|
  config.vm.box = "centos/stream8"
  config.vm.box_version = "20210210.0"
end
```
2. install vagrant vbguest addon
```shell
$ vagrant plugin install vagrant-vbguest
```
3. start the base image
```shell
$ vagrant up
```
4. if the vbox-addon installation fails, ssh into the base image
```shell
$ vagrant ssh
#install kernel headers
(base-image)# dnf install -y kerne-devel
(base-image)# exit
$ vagrant vbguest --do install
```

6. Once the base image has required files, create box
```shell
vagrant package --output D:\Users\xxx\path\to\box\centos8stream-vbox_guest.box
vagrant box add --name="Centos8Stream-vbox-guest" D:\Users\xxx\path\to\box\centos8stream-vbox_guest.box
```

## Usage/Examples

To provision the cluster, execute the following commands.

```shell
git clone https://github.com/bharat555/vagrant-kubeadm-kubernetes.git
cd vagrant-kubeadm-kubernetes
# edit Vagrantfile to use the correct image with vbguest addons
config.vm.box = "Centos8Stream-vbox-guest"
vagrant up
```

## Set Kubeconfig file varaible.

```shell
cd vagrant-kubeadm-kubernetes
cd configs
export KUBECONFIG=$(pwd)/config
```

or you can copy the config file to .kube directory.

```shell
cp config ~/.kube/
```

## Kubernetes Dashboard URL

```shell
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/overview?namespace=kubernetes-dashboard
```

## Kubernetes login token

Vagrant up will create the admin user token and saves in the configs directory.

```shell
cd vagrant-kubeadm-kubernetes
cd configs
cat token
```

## To shutdown the cluster, 

```shell
vagrant halt
```

## To restart the cluster,

```shell
vagrant up
```

## To destroy the cluster, 

```shell
vagrant destroy -f
```

  
