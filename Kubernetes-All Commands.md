# How to install docker and kubernetes in ubuntu

## install openssh-server 
    $ sudo apt-get install openssh-server
    $ sudo service ssh status 
    $ ssh localhost
    
## install Docker

    $ sudo apt-get update
    
    $ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
 
    $ echo \
      "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 


## Install Docker 
      
      $ sudo apt-get update
      
      $ sudo apt-get install docker-ce docker-ce-cli containerd.io
    
      $ apt-cache madison docker-ce
      
   
## Test Dockers

          $ sudo docker run hello-world
      
## Installing kubeadm 

### Letting iptables see bridged traffic (Note: Please copy the below command from Kuberenetes website)

        cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
        br_netfilter
        EOF

        cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
        net.bridge.bridge-nf-call-ip6tables = 1
        net.bridge.bridge-nf-call-iptables = 1
        EOF
        sudo sysctl --system
     
### Update the apt package index and install packages needed to use the Kubernetes apt repository:

      $ sudo apt-get update

      $ sudo apt-get install -y apt-transport-https ca-certificates curl
      
      $ sudo ufw disable
      
      $ swapoff -a

  ## Go to Step 2. Steps to install Kubernetes

--------------------------------------------------------------------------------------
      
# How to install docker and kubernetes in centos 

    $  systemctl stop firewalld

    $  systemctl disable firewalld

    $  yum install -y yum-utils

    $  yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo
 
    $  yum install docker-ce docker-ce-cli containerd.io
 
    $  systemctl start docker
 
    $  systemctl enable docker
    
      cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
      br_netfilte
      EOF  

       cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
       net.bridge.bridge-nf-call-ip6tables = 1
        net.bridge.bridge-nf-call-iptables = 1
        EOF
  
        $ sudo sysctl --system
   
        $  cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
           [kubernetes]
           name=Kubernetes
           baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
           enabled=1
           gpgcheck=1
           repo_gpgcheck=1
           gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
           exclude=kubelet kubeadm kubectl
           EOF


            $  sudo setenforce 0

            $  sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

            $  sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

            $  sudo systemctl enable --now kubelet

            $  yum update

            $  exit

            $  sudo sed -i "/^[^#]*PasswordAuthentication[[:space:]]no/c\PasswordAuthentication yes" /etc/ssh/sshd_config

            $  sudo service sshd restart

            $  history

---------------------------------------------------------

## 2. Steps to install Kubernetes 

            $ kubeadm init --pod-network-cidr=192.168.0.0/16

### switch off swap memory and every time the system reboots the swap memory is enabled again. 

            $ swapoff -a

            $ sudo vi  /etc/fstab

            $ kubeadm token create --print-join-command

            $ kubectl get pod --all-namespaces

            $ docker ps


 
## install calico cni Plug-in, this allows communication between master and Worker nodes.----------( https://docs.projectcalico.org/getting-started/kubernetes/quickstart  )

         $ kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml

         $ kubectl create -f https://docs.projectcalico.org/manifests/custom-resources.yaml	

## Basic Commands for Kubernetes
         $ kubectl run my-first-pod --image stacksimplify/kubenginx:1.0.0   or   $ kubectl run my-first-pod(nameof the pod) --image myown.registery.com/
         stacksimplify/kubenginx:1.0.0

         $ kubectl get pod

         $ kubectl get po

         $ kubectl get pods -o wide

### to get more information about any of the service in kubernetes
 
         $ kubectl describe pod my-first-pod
 
### How to expose an application outisde k8s cluster
 
         $ kubectl get svc

         $ kubectl get pod

         $ kubectl expose pod my-first-pod --type=NodePort --port=80 --name=my-first-service

         $ kubectl get svc
 
 ### After it is Exposed please go to Google crom and type your worker node IP and : then port of my-first-service
 
### how to deploy 
 
         $  yum install git -y

         $  git clone https://github.com/gopal1409/ictdeployment.git

         $  ls

         $  cd ictdeployment/

         $  kubectl apply -f deployment.yml

         $ kubectl get deploy

         $ kubectl get rs

         $ kubectl get pod

         $ kubectl describe deploy

         $ kubectl describe rs

### change the image
 
         $ kubectl set image deployment/myapp3-deployment myapp3-container=stacksimplify/kubenginx:2.0.0 --record

         $ kubectl get rs

         $ kubectl get pod

         $ kubectl set image deployment/myapp3-deployment myapp3-container=stacksimplify/kubenginx:1.0.0 --record

         $ kubectl get deploy

### to go back and froth in deployment
 
         $ kubectl describe rs

         $ kubectl edit rs 

         $ kubectl edit pod  

         $ kubectl edit deploy

         $ kubectl get deploy

         $ kubectl rollout status myapp3-deploy

         $ kubectl rollout status myapp3-deployment

         $ kubectl rollout history deployment.v1.apps/nginx-deployment

         $ kubectl rollout history deployment.v1.apps/myapp3-deployment

         $ kubectl rollout undo deployment.v1.apps/myapp3-deployment

         $ kubectl rollout undo deployment.v1.apps/myapp3-deployment --to-revision=1
 
# Day 2- Kubernetes for Day 2


## this should be Run on only the First Machine 
         $ su - 
 
         $ kubeadm init --pod-network-cidr=192.168.0.0/16

## install calico pugin
 
         $ kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml

         $ kubectl create -f https://docs.projectcalico.org/manifests/custom-resources.yaml
 
## how to expose a deployment
         $  kubectl expose deployment myapp3-deployment --type=NodePort --name=myapp3

         $  kubectl get svc
### how to check the service
 
         $ kubectl get svc myapp3

         $ kubectl describe svc myapp3

         $ kubectl get pod -o wide
  
### rolling back and forth
 
         $ kubectl set image deployment/myapp3-deployment myapp3-container=piuma/phpsysinfo --record

         $ kubectl set image deployment/myapp3-deployment myapp3-container=stacksimplify/kubenginx:2.0.0 --record


          $ kubectl rollout history deployment.v1.apps/myapp3-deployment

         $ kubectl rollout history deployment.v1.apps/myapp3-deployment --revision=2

         $ kubectl rollout history deployment.v1.apps/myapp3-deployment --revision=3

         $ kubectl get deploy

         $ kubectl edit deploy myapp3-deployment

         $ kubectl get deploy

         $ kubectl rollout undo deployment.v1.apps/myapp3-deployment --to-revision=4

## How to deploy a dashboard in kubernetes
 
         $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml

         $ kubectl get svc --all-namespaces

         $ kubectl edit svc kubernetes-dashboard --namespace=kubernetes-dashboard

            then press Esc button and i
            then go to the second last line
            change the clusterIp to NodePort
            then Press Esc :wq
    
###  then again do 
   
           $ kubectl get svc --all-namespaces
### get the port number of kubernetes dashboard
   
          nodemachineip:portnumber

## Create ServiceAccount and role

            $ cd ictdeployment

            $ git pull

            $ kubectl apply -f role.yml

            $ kubectl apply -f serviceaccount.yml

            $ kubectl get serviceaccount --all-namespaces

### Command to generate token

            $ kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}"

## PV and PVC

### login to second machine

            $ mkdir /mnt/data/

### ON master

            $  cd ictdeployment

            $  git pull

            $  kubectl apply -f pv.yml

            $  kubectl get pv

            $  kubectl apply -f pvc.yaml

            $  kubectl get pv

## in cloud use azure blob storage 
            $ kubectl get sc
            $ kubectl create sc azurepremiumstorage 

## Configmap

            $ git pull  (go to folder ictdeployment)

            $ kubectl apply -f userconfig.yml

            $ kubectl get configmap

            $ kubectl describe configmap usermanagement-dbcreation-script

            $ kubectl apply -f mysql.yml

**to verify** 

### go to node machine

            $ cd /mnt/data

## check sql files

## On Master
            $ kubectl get pod

            $ kubectl exec -it mysql-5d978846bd-n7cqt -- /bin/bash       # use your MySQL container name

### once you log inside the container

            $ mysql -u root -p

### password is dbpassword11

            $ show databases;

            $ git pull

            $ kubectl apply -f mysqlcluster.yml

            $ kubectl apply -f front.yml

            $ kubectl get deploy

            $ kubectl describe deploy usermgmt-webapp

            $ kubectl expose deploy usermgmt-webapp --type=NodePort --port=8080 --name=frontservice

### Connect with your your worker node IP and frontservices port number then when you get the site use Username: admin101 and Password: password101

### On Master

            $ kubectl get pod

            $ kubectl exec -it mysql-5d978846bd-n7cqt -- /bin/bash       # use your MySQL container name

## once you log inside the container

            $ mysql -u root -p

## password is dbpassword11
        > show databases;

        > use webappdb

        > show tables;

        > select * from user;

        > exit

---------------------------------------------------------

# Day 3 Kubernetes

## Helm  Chart 

        $ yum install wget

        $ wget https://get.helm.sh/helm-v3.5.4-linux-amd64.tar.gz

        $ tar zxvf helm-v3.5.4-linux-amd64.tar.gz

        $ cd linux-amd64/

        $ ls

        $ mv helm /usr/bin/

        $ cd /

        $ helm

        $ helm repo add stable https://kubernetes-charts.storage.googleapis.com

        $ helm repo add bitnami https://charts.bitnami.com/bitnami

------------------------------------------------------------
## Prometheus Install 

            $ helm install my-release bitnami/kube-prometheus

            $ kubectl edit svc my-release-kube-prometheus-prometheus

            $ Kubectl get svc

----------------------------------------------
## Grafana install 

            $ helm install my-grafana bitnami/grafana-operator

### you your own name in palce of my-grafana

            $ Kuberctl get svc

-----------------------------------
            $ helm list

            $ helm uninstall my-grafana

            $ helm uninstall my-release

            $ helm repo add stable https://charts.helm.sh/stable

            $ helm install prometheus-operator stable/prometheus-operator

            $ kubectl get pod

            $ kubectl get svc

            $ kubectl edit svc prometheus-operator-grafana

            $ kubectl get svc

### use worker node or master node IP which ever works for you with prometheus-operator-grafana Port to open on Crome.
## below are crediantials for grafana

    username: admin

    password: prom-operator

---------------------------------------------------------

**when you create an deplyoument file you need to add this.** 

    https://www.base64encode.org/
    echo "YWRtaW4=" | base64 --decode echo "cHJvbS1vcGVyYXRvcgo=" | base64 --decode

---------------------------------------------------------

### How you create a secret and hide your password which is in plain txt in mysql 

        $ git pull

        $ kubectl apply -f secret.yml

        $ kubectl apply -f mysql.yml

        $ kubectl apply -f front.yml

        $ kubectl describe secret my-sql-db-password

---------------------------------------------------------

### Suppose you want to upgrade the cluster from one version to another

### First you need to drain your master

            $ kubectl get nodes

            $ kubectl drain ip-172-31-11-21.us-east-2.compute.internal

            $ kubectl drain ip-172-31-11-21.us-east-2.compute.internal --ignore-daemonsets

            $ kubectl drain ip-172-31-11-21.us-east-2.compute.internal --delete-emptydir-data

            $ kubectl drain ip-172-31-11-21.us-east-2.compute.internal --delete-emptydir-data  --ignore-daemonsets

            $ kubectl get nodes

            $ yum show kubeadm

            $ show kubeadm

            $ yum kubeadm

            $ yum install kubeadm

            $ kubeadm upgrade plan

            $ kubeadm upgrade apply

            $ kubectl get nodes

            $ kubectl uncordon ip-172-31-11-21.us-east-2.compute.internal

            $ kubectl get nodes

---------------------------------------------------------


### Whenever you are using a container always see that It talk to your kernal  

            $ kubectl run pod --image=nginx 

            $ kubectl exec pod -it nginx -- /bin/bash 
            Inside your container uname -r


---------------------------------------------------------

## Crictl : currently we are running docker as an engine later on we will use other container service crio or virtlet

### To install crictl

            $ wget https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.21.0/crictl-v1.21.0-linux-amd64.tar.gz

            $ tar zxvf crictl-v1.21.0-linux-amd64.tar.gz

            $ mv crictl /usr/bin/

---------------------------------------------------------

## Create a namespace istio-system for Istio components:

            $ kubectl create namespace istio-system

## Install the Istio base chart which contains cluster-wide resources used by the Istio control plane:

            $ helm install istio-base manifests/charts/base -n istio-system

### Install the Istio discovery chart which deploys the istiod service:

            $ helm install istiod manifests/charts/istio-control/istio-discovery -n istio-system

## (Optional) Install the Istio ingress gateway chart which contains the ingress gateway components:

            $ helm install istio-ingress manifests/charts/gateways/istio-ingress -n istio-system

## (Optional) Install the Istio egress gateway chart which contains the egress gateway components:

            $ helm install istio-egress manifests/charts/gateways/istio-egress -n istio-system

---------------------------------------------------------


