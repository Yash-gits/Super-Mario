**Setting up EC2 resources.**

1. Launch an EC2 Instance with the following requirements:
2. Name provided: Super-Mario
3. AMI: Ubuntu/Linux
4. Instance type: t2.Medium [Having 2 CPUs, 4Gi Memory]
5. Provided the keypair and Enabled the security group for all inbound traffic.[for the learning purpose]
6. Configure storage as required we can take upto 30Gis of EBS General Purpose (SSD) or Magnetic storage [For free tier]
7. After launching ther instance the interface we'll get is

 ![image](https://github.com/user-attachments/assets/07fcf06a-2cad-449b-9cc9-fda93380e258)
8. We will now create an IAM role with allowing the required permissions for EKS AND EC2. and Setup this role for the abolve instance.
![image](https://github.com/user-attachments/assets/ef507645-6867-4314-8973-a0af9ebbead3)

Permission policies allowed are:

![image](https://github.com/user-attachments/assets/45464874-7929-49af-83ba-2d43a2fd8c99)

9. We will now get the access to our instance using SSH, I am here using Mobaxterm.
10. Logged in to killercoda and write the following data in service. yaml file

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mario-service
spec:
  type: LoadBalancer
  selector:
    app: mario
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```
11. After creating the service.yaml file it should look something like this:

![image](https://github.com/user-attachments/assets/bba727ad-76df-49bb-91cc-99760c11ec59)

12. Let's setup the deployment.yaml file

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mario-deployment
spec:
  replicas: 2  # We can adjust the number of replicas as we require.
  selector:
    matchLabels:
      app: mario
  template:
    metadata:
      labels:
        app: mario
    spec:
      containers:
      - name: mario-container
        image: sevenajay/mario:latest #[Taken the Mario image from the public repositary]
        ports:
        - containerPort: 80
```
13. After successfully creating the deployment.yaml file we can use the following command to list all:

![image](https://github.com/user-attachments/assets/8e6bfc60-1e68-4ef8-8ef4-46796ab10893)

14. Creating a "script.sh" file for installing "terraform, Kubectl, AWS Cli".

```yaml
#!/bin/bash
# Install Terraform
sudo apt install wget -y
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform -y

# Install kubectl
sudo apt update
sudo apt install curl -y
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client

# Install AWS CLI 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt-get install unzip -y
unzip awscliv2.zip
sudo ./aws/install



echo "Installation completed successfully."

```




