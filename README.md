# OpenShift Cluster Setup 

* OpenShift is Kubernetes Cloud based Container Platfrom. It leverage kubernetes with aditional features and tools which make is easier  to deploy, manage, and scale Container. Some of key features are build automation, CI/CD and many more.
* Openshift have Command line interface[CLI] like kubernetes Kubectl called "oc" and it also provide Graphical User Interface [GUI]. 
* Here Will will be creating cluster with Azure cloud User defined.
### Creating Developer Account in Red Hat
You need Red Hat Developer account for access to openshift as it is commercial product developed by Red Hat 

You can create account [here](https://developers.redhat.com)

* Register Domain and intergrate it with Azure DNS.
* Create Azure VM machine and login to it and install Azure CLI in it.

### Setup Openshift Cluster Using User Provisioned Infrastructure
 
#### 1. Generating Key Pair For Cluster Node SSH Access:
``` 
sudo -i
ssh-keygen -t ed25519 -N '' -f ~/.ssh/id_rsa
```
#### 2. Obtaining the Installation Program
* Access the Infrastructure Provider page on the Red Hat OpenShift Cluster Manager site. If you have a Red Hat account, log in with your credentials. If you do not, create an account. To create an account follow the Pre-requisite section Subscription to Redhat.
* To open Infrastructure Provider Click [Here](https://console.redhat.com/openshift/create)
* Let's Create Dir for all installation
```
sudo su
mkdir /root/openshift
cd /root/openshift
```
* Extract the installation program
```
wget <Link of openshift program>
tar xvf openshift-install-linux.tar.gz
```
* Install OpenShift CLI on Linux:
```
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/stable/openshift-client-linux.tar.gz
```
It Will install `oc` and `kubectl` 
```
chmod +x kubectl
mv kubectl /usr/bin
chmod +x oc
mv oc /usr/bin
```
###### Before Installing Cluster Create Azure `Princpal client id` and `Princpal client secret` by registering New App Registration in Azure Active Directory

#### OPENSHIFT CLUSTER INSTALLATION
##### Start Openshift Custer Installation

* First we will create install-config file:
```
 ./openshift-install create install-config --dir=/root/openshift
```
* After running above command it will ask for:
    - Platfrom like Azure , AWS, IBM cloud, etc. Select `Azure`
    - Azure Subscription Id
    - Azure Tenant Id
    - Azure Service Principal Client Id
    - Azure Service Principal Client Secret

* Now we will install cluster:
```
./openshift-install create cluster --dir=/root/openshift/ --log-level=info
```

`Wait for 30-45 min with Terraform scripts your infrastructure will be  created.`

#### Logging into Cluster 

* Export the kubeadmin credentials from the root user
```
export KUBECONFIG=/rootopenshift/auth/kubeconfig
echo 'export KUBECONFIG=/rootopenshift/auth/kubeconfig' >> $HOME/.bashrc
```

* To Verify if nodes are running using `oc` 
```
oc get nodes
```
* Same With `kubectl`
```
kubectl get nodes
``` 

#### Checking Cluster Logs
You can review a summary of an installation in the OpenShift Container Platform installation log. If an installation succeeds, the information required to access the cluster is included in the log.

with sudo privilages
```
cat /root/openshift/.openshift_install.log
```
#### Delete Openshift Cluster

For deleting the cluster Run Below Commands
```
sudo su
cd /root/openshift
```
```
./openshift-install destroy cluster --dir=/root/openshift --log-level=info
```
### Conclusion

There are many ways to deploy an application on OpenShift. In this post, I discussed and showed some of the easiest methods on how to deploy an application on OpenShift OCP? If you already have all those tools that build and store images, then you can deploy applications using container images. Hence, OpenShift is a great tool for container-based applications over the cloud. We can build, test, deploy and maintain applications using it very easily.
