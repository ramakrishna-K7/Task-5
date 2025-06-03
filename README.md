                        TASK 5: Build a Kubernetes Cluster Locally with Minikube

STEP 1: Launch EC2 Instance
•	AMI: Amazon Linux (Kernel 5.10)
•	Instance Type: t2.medium
•	EBS Volume: 8GB
________________________________________
STEP 2: Install Docker, Minikube and kubectl
yum install docker -y
systemctl start docker
systemctl status docker

# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo mv kubectl /usr/local/bin/kubectl
sudo chmod +x /usr/local/bin/kubectl

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Install dependencies
sudo yum install iptables -y
yum install conntrack -y

# Start Minikube with Docker driver
minikube start --driver=docker --force

# Check Minikube status
minikube status

________________________________________
STEP 3: Create Kubernetes Deployment and Service files
deployment.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: thunder
spec:
  replicas: 2
  selector:
    matchLabels:
      app: sky
  template:
    metadata:
      labels:
        app: sky
    spec:
      containers:
        - name: cloud
          image: nginx
          ports:
            - containerPort: 80
            

service.yml

apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: sky
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30001
________________________________________
STEP 4: Apply the Kubernetes Configurations
kubectl apply -f deployment.yml
kubectl apply -f service.yml
________________________________________
STEP 5: Verify Deployment and Service

Check Service
kubectl get svc
output:
![image](https://github.com/user-attachments/assets/0d9cdc9b-132f-404b-b066-b540d79e1729)
 

Check Pods Before Scaling Up
kubectl get po
output:
![image](https://github.com/user-attachments/assets/902fe19f-6580-4204-9095-924ca90e9bdc)
 

Check Pods After Scaling Up
kubectl scale deployment thunder --replicas=4
kubectl get po
output:
 ![image](https://github.com/user-attachments/assets/a065a48d-9a63-4ffd-b50f-a4916040fd24)


Check the logs usging kubectl log
kubectl logs <pod-name>
________________________________________
RESULT

![image](https://github.com/user-attachments/assets/5a3a8db8-b403-4483-a6f2-fc8c87bd68f7)
 
