Jenkins Configuration:

Java 11 ( open JDK )
sudo add-apt-repository ppa:openjdk-r/ppa 
sudo apt-get update 
sudo apt install openjdk-11-jdk 

Jenkins installation in ubuntu
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
Start Jenkins
You can enable the Jenkins service to start at boot with the command:
sudo systemctl enable jenkins
You can start the Jenkins service with the command:
sudo systemctl start jenkins
You can check the status of the Jenkins service using the command:
sudo systemctl status jenkins
If everything has been set up correctly, you should see an output like this:
Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
Active: active (running) since Tue 2018-11-13 16:19:01 +03; 4min 57s ago


Post-installation setup wizard
After downloading, installing and running Jenkins using one of the procedures above (except for installation with Jenkins Operator), the post-installation setup wizard begins.
This setup wizard takes you through a few quick "one-off" steps to unlock Jenkins, customize it with plugins and create the first administrator user through which you can continue accessing Jenkins.
Unlocking Jenkins
When you first access a new Jenkins instance, you are asked to unlock it using an automatically-generated password.
1. Browse to http://localhost:8080 (or whichever port you configured for Jenkins when installing it) and wait until the Unlock Jenkins page appears.

2. From the Jenkins console log output, copy the automatically-generated alphanumeric password (between the 2 sets of asterisks).

Note:
The command: sudo cat /var/lib/jenkins/secrets/initialAdminPassword will print the password at console.
If you are running Jenkins in Docker using the official jenkins/jenkins image you can use sudo docker exec ${CONTAINER_ID or CONTAINER_NAME} cat /var/jenkins_home/secrets/initialAdminPassword to print the password in the console without having to exec into the container.
3. On the Unlock Jenkins page, paste this password into the Administrator password field and click Continue.
Notes:
You can always access the Jenkins console log from the Docker logs (above).
The Jenkins console log indicates the location (in the Jenkins home directory) where this password can also be obtained. This password must be entered in the setup wizard on new Jenkins installations before you can access Jenkins’s main UI. This password also serves as the default administrator account’s password (with username "admin") if you happen to skip the subsequent user-creation step in the setup wizard.





Sonar installation

Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.
Set up the repository
1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:
 sudo apt-get update
 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
Add Docker’s official GPG key:
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below. Learn about nightly and test channels.
3.  echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Install Docker Engine
1. Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
Got multiple Docker repositories?
If you have multiple Docker repositories enabled, installing or updating without specifying a version in the apt-get install or apt-get update command always installs the highest possible version, which may not be appropriate for your stability needs.

docker run -d -p 9000:9000 sonarqube:lts

sonarPassword: Think@123


Configuring the Nexus

JAVA 8 installation in ubuntu 16.04
sudo apt-get -y install default-jdk 
Nexus Installation in ubuntu 16.04
apt-get install wget ( install if you dont have wget ) 
java -version ( make sure java is installed which should be java 8 or higher version ) 
wget https://sonatype-download.global.ssl.fastly.net/nexus/3/nexus-3.0.2-02-unix.tar.gz
tar -xvf latest-unix.tar.gz 
cd nexus-3.35.0-02/bin 
./nexus start ( starts the nexus artifactory ) 
./nexus status ( by this you check the status of nexus artifactory ) 
To access this use http://ip_Address:8081 ( by deafault which will be running on 8081) 
intial password will be present in /opt/sonatype-work/nexus3/admin.passw
useradd -M -d /opt/nexus -s /bin/bash -r nexus
echo "nexus   ALL=(ALL)       NOPASSWD: ALL" > /etc/sudoers.d/nexus


installing kubeadm
Install kubelet, kubeadm and kubectl
add Kubernetes repository for Ubuntu 20.04 to all the servers.
sudo apt update
sudo apt -y install curl apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
Then install required packages.
sudo apt update
sudo apt -y install vim git curl wget kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
Confirm installation by checking the version of kubectl.

kubectl version --client && kubeadm version
Client Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.2", GitCommit:"8b5a19147530eaac9476b0ab82980b4088bbc1b2", GitTreeState:"clean", BuildDate:"2021-09-15T21:38:50Z", GoVersion:"go1.16.8", Compiler:"gc", Platform:"linux/amd64"}
kubeadm version: &version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.2", GitCommit:"8b5a19147530eaac9476b0ab82980b4088bbc1b2", GitTreeState:"clean", BuildDate:"2021-09-15T21:37:34Z", GoVersion:"go1.16.8", Compiler:"gc", Platform:"linux/amd64"}

Disable Swap
Turn off swap.
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
sudo swapoff -a
Enable kernel modules and configure sysctl.
Enable kernel modules
sudo modprobe overlay
sudo modprobe br_netfilter
Add some settings to sysctl
sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
Reload sysctl
sudo sysctl --system
Install Container runtime
Installing Docker runtime:
Add repo and Install packages
sudo apt update
sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install -y containerd.io docker-ce docker-ce-cli

Create required directories
sudo mkdir -p /etc/systemd/system/docker.service.d
Create daemon json config file
sudo tee /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
Start and enable Services
sudo systemctl daemon-reload 
sudo systemctl restart docker
sudo systemctl enable docker
Ensure you load modules
sudo modprobe overlay
sudo modprobe br_netfilter
Set up required sysctl params
sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
Reload sysctl
sudo sysctl --system
kubeadm init --ignore-preflight-errors=all


Initialize master node
Login to the server to be used as master and make sure that the br_netfilter module is loaded:
lsmod | grep br_netfilter
Enable kubelet service.
sudo systemctl enable kubelet
Initialize kubeadm
kubeadm init
Configure kubectl using commands in the output:
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
Additional nodes can be added using the command in installation output:
kubeadm join k8s-cluster.computingforgeeks.com:6443 --token sr4l2l.2kvot0pfalh5o4ik \
    --discovery-token-ca-cert-hash sha256:c692fb047e15883b575bd6710779dc2c5af8073f7cab460abd181fd3ddb29a18 \
    --control-plane
Install network plugin on Master
In this we’ll use Calico. You can choose any other supported network plugins.
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

