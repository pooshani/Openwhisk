# OpenWhisk deployment on a Kubernetes Cluster
 ## Prerequisites: Docker and Helm
  ### 1- install Docker
    # Add Docker's official GPG key:
    sudo apt-get update
    sudo apt-get install ca-certificates curl GnuPG 
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    
    # Add the repository to Apt sources:
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update

    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
 
### 2- install Helm
    wget https://get.helm.sh/helm-v3.13.3-linux-amd64.tar.gz
    tar -zxvf helm-v3.13.3-linux-amd64.tar.gz
    mv linux-amd64/helm /usr/local/bin/helm
  ## Configuration steps for Openwhisk
  ### 1- Cluster configuration file setup 
   Next, you need to create a .yaml file to describe your cluster. An example of such a file can be found at openwhisk/mycluster.yaml. You need to replace <master_node_public_IP> with the public IP address of the master node in the cluster.

### install OpenWhisk Command-line Interface (WSK)

     wget https://github.com/apache/openwhisk-cli/releases/download/latest/OpenWhisk_CLI-latest-linux-386.tgz
     tar -xvf OpenWhisk_CLI-latest-linux-386.tgz
     sudo mv wsk /usr/local/bin/wsk

### Setting up necessary files

## Deployment of OpenWhisk
 Firstly, we need to specify which of our nodes are core which operates the OpenWhisk control plane (the controller, kafka, zookeeeper, and couchdb pods) and invoker nodes which schedules and executes user containers.
 Label all nodes as part of the cluster as Invoker nodes by running this command:
 
     kubectl label nodes --all openwhisk-role=invoker
 then Deploy OpenWhisk using the following commands:
 
     kubectl create namespace openwhisk
     git clone https://github.com/apache/openwhisk-deploy-kube.git
     cd openwhisk-deploy-kube
     mv ../mycluster.yaml .
     helm install owdev ./helm/openwhisk -n openwhisk --create-namespace -f mycluster.yaml

 and Configure the OpenWhisk CLI, wsk, by setting the auth and apihost properties:
 
     wsk  property  set  --apihost <master_node_public_IP>
     wsk  property  set  --auth  23bc46b1-71f6-4ed5-8c54-816aa4f8c502:123zO3xZCLrMN6v2BKK1dXYFpXlPkccOFqm12CdAsMgRU4VrNZ9lyGVCGuMDGIwP

You can use the following command to monitor the pod creation process:

     kubectl  get  pods  -n  openwhisk  --watch
    
and  Check if it working or not by listing the installed packages:

     wsk -i package list /whisk.system
