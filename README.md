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
