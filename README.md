# $$\color{green}{\textbf Project: 🎮 \color{red} \textbf {Super} \ \color{orange} \ \textbf Mario  \ \textbf Bros 🍄🐢}$$

##  $\color{blue} \textbf {Project  Workflow}$
Step 1 → Login and basics setup

Step 2 → Setup Docker  ,aws cli ,eksctl and Kubectl

Step 3 → IAM Role for EC2

Step 4 →Attach IAM role with your EC2

Step 5 → Building Infrastructure Using kubernetes

Step 6 → Creation of deployment and service for EKS



### $\color{red} \textbf {Step 1 → Login  and  basics  setup}$
1. Click on launch Instance
   ![instance](https://github.com/abhipraydhoble/Project-Super-Mario/assets/122669982/5fe51373-eaac-4f7c-9669-34c578277051)
2. Connect to EC2-Instance
   ![connect-ec2](https://github.com/abhipraydhoble/Project-Super-Mario/assets/122669982/9d518e77-6f65-4153-acfc-790a6eaf669a)

   
5. Attach role to ec2 instance

### $\color{red} \textbf {Step 2 → Setup  Tools}$

````
sudo apt update -y
````
$\color{blue} \textbf {Setup  Docker:}$
````
sudo apt install docker.io
sudo systemctl start docker
sudo usermod -aG docker ubuntu
newgrp docker
docker --version
````
${\color{blue} \textbf {Setup  AWS CLI:}}$
````
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip 
unzip awscliv2.zip
sudo ./aws/install
aws --version

````

## ${\color{blue} \textbf {Install kubectl}}$
Download the latest release with the command:
````
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
````
Validate the binary 
````
 curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
````
Validate the kubectl binary against the checksum file:
````
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
````
Install kubectl:
````
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
````
Note:
If you do not have root access on the target system, you can still install kubectl to the ~/.local/bin directory:
````
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
````
````
kubectl version --client
````
### $\color{red} \textbf {Step 3 → IAM  Role  for  EC2}$
create role:
![role](https://github.com/abhipraydhoble/Project-Super-Mario/assets/122669982/31a05c18-f34b-430d-b5cb-c5873ae6e9c5)

### $\color{red} \textbf {Step 4 →Attach  IAM  role  with your  EC2 }$
go to EC2 
click on actions → security → modify IAM role option
- administrator access
- eks
![image](https://github.com/user-attachments/assets/c23f9d00-505d-4a0d-b07d-c6b21d419748)

![role-ec2](https://github.com/abhipraydhoble/Project-Super-Mario/assets/122669982/70cc0ebb-6063-4c4b-98df-7259a08749b8)

![modify-role](https://github.com/abhipraydhoble/Project-Super-Mario/assets/122669982/3e998e21-3654-43b0-8df0-496f009ef0a6)

### $\color{red} \textbf {Step 5 → Building Infrastructure  Using  kubernetes}$
$\color{blue} \textbf {Install  GIT}$
````
Aws configure --profile eks
sudo apt install git -y
git clone https://github.com/abhipraydhoble/Project-Super-Mario.git
cd Project-Super-Mario
cd EKS-TF
vim backend.tf
````
![backend tf](https://github.com/abhipraydhoble/Project-Super-Mario/assets/122669982/6b9e648f-2f13-41e8-a66b-6b6e6e0a63de)

$\color{blue} \textbf {Create \ Infra:}$ install eksctl 
````
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
sudo chmod +x /usr/local/bin/eksctl
eksctl version
eksctl create cluster \
  --name eks-cloud \
  --region ap-south-1 \
  --nodegroup-name my-nodes \
  --node-type t3.medium \
  --nodes 1 \
  --nodes-min 1 \
  --nodes-max 2 \
  --managed


aws eks update-kubeconfig --name EKS_CLOUD --region ap-south-1
````
or
````
aws eks update-kubeconfig --name EKS_CLOUD --profile eks
````

### $\color{red} \textbf {Step 6 → Creation  of  deployment  and service  for  EKS}$
change the directory where deployment and service files are stored use the command →
````
cd ..
````
$\color{blue} \textbf {create  the  deployment}$
````
kubectl apply -f deployment.yaml
````
$\color{blue} \textbf {Now create  the service}$
````
kubectl apply -f service.yaml
kubectl get all
kubectl get svc mario-service
````
copy the load balancer ingress and paste it on browser and your game is running

![load balancer](https://github.com/abhipraydhoble/Project-Super-Mario/assets/122669982/d085951d-3398-44ad-b9cd-05c561b74664)



$\color{green} \textbf {Final Output: Enjoy The Game 🎮}$

![output](https://github.com/abhipraydhoble/Project-Super-Mario/assets/122669982/edfff0b5-6507-48e4-b552-908671b59920)

**Delete Infra**
````
 terraform destroy -auto-approve
````

