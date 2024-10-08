**Setting up EC2 resources.**

1. Launch an EC2 Instance with the following requirements:
2. Name provided: Super-Mario
3. AMI: Ubuntu/Linux
4. Instance type: t2.Medium [Having 2 CPUs, 4Gi Memory]
5. Provided the keypair and Enabled the security group for all inbound traffic.[for the learning purpose]
6. Configure storage as required we can take upto 30Gis of EBS General Purpose (SSD) or Magnetic storage [For free tier]
7. After launching ther instance the interface we'll get is
 ![image](https://github.com/user-attachments/assets/07fcf06a-2cad-449b-9cc9-fda93380e258)
8. We will now get the access to our instance using SSH, I am here using Mobaxterm.
9. Logged in to killercoda and write the following data in service. yaml file

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
![image](https://github.com/user-attachments/assets/eaff02c5-279e-4364-a703-cbf6bad2e8cb)

