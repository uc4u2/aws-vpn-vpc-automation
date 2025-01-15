# **AWS Law Firm Cloud Setup with OpenVPN**

## **Overview**
This project provides an AWS CloudFormation template to set up a Virtual Private Cloud (VPC) integrated with an OpenVPN server. Designed for small law firms, this solution ensures secure remote access, scalable infrastructure, and reliable network management. The setup is customizable for other small businesses requiring similar cloud-based VPN solutions.

---

## **Features**
- **Automated AWS VPC Setup**:
  - Public and private subnets.
  - NAT Gateway for secure internet access from private subnets.
  - Internet Gateway for public access.

- **OpenVPN Integration**:
  - OpenVPN server deployed on an EC2 instance.
  - Secure remote access for employees using OpenVPN clients.

- **Scalability and Security**:
  - Designed to support multiple users with easy client configuration management.
  - AWS Security Groups for controlled access to resources.

- **Customizability**:
  - Supports modifications for IP ranges, instance types, and number of users.

---

## **Architecture**
![Architecture Diagram](assets/architecture-diagram.png)

### **Components**
1. **VPC**: Contains public and private subnets for secure resource segmentation.
2. **Subnets**:
   - Public Subnet: Hosts the OpenVPN server and other public-facing resources.
   - Private Subnet: For internal services and data storage.
3. **OpenVPN Server**:
   - Hosted on a t2.micro EC2 instance.
   - Configured to provide secure VPN access.
4. **Security Groups**:
   - Allows SSH and OpenVPN connections.
5. **Outputs**:
   - VPC ID, Subnet IDs, and OpenVPN server public IP address.

---

## **Deployment Instructions**

### **Prerequisites**
1. **AWS Account**: Ensure you have an active AWS account.
2. **Key Pair**: Create an AWS EC2 key pair (e.g., `2xKeyPair`) for SSH access.
3. **CloudFormation Console**: Familiarity with AWS CloudFormation.

### **Steps**
1. **Clone this Repository**:
   ```bash
   git clone https://github.com/your-username/aws-law-firm-project.git
   cd aws-law-firm-project
   ```

2. **Launch the Stack**:
   - Navigate to the AWS CloudFormation console.
   - Upload the `lawfirm-vpc.yaml` file.
   - Provide parameters such as `VpcName`, `CidrBlock`, and `InstanceKeyName`.

3. **Access the OpenVPN Server**:
   - SSH into the server using the key pair.
   - Use the OpenVPN client configuration files (`client1.ovpn`, `client2.ovpn`, etc.) to connect.

4. **Connect Clients**:
   - Install the OpenVPN client on user devices.
   - Import the `.ovpn` configuration file and connect.

---

## **Managing Users**

### **Adding New Users**
Run the following commands on the OpenVPN server to create additional client configuration files:
```bash
cd /usr/share/easy-rsa
sudo ./easyrsa build-client-full client3 nopass
sudo cp /etc/openvpn/client1.ovpn /etc/openvpn/client3.ovpn
sudo nano /etc/openvpn/client3.ovpn  # Update certificate and key paths
```
Distribute the updated `.ovpn` file along with the necessary certificates to the user.

### **Revoking Users**
To revoke access for a user:
```bash
cd /usr/share/easy-rsa
sudo ./easyrsa revoke client3
sudo ./easyrsa gen-crl
sudo systemctl restart openvpn@server
```
---

## **Costs and Optimization**
- **AWS Free Tier**:
  - 750 hours of t2.micro or t3.micro instances per month.
  - 1GB outbound traffic per month.
- **Tips for Cost Efficiency**:
  - Monitor traffic using AWS CloudWatch.
  - Automate client management to save administrative overhead.

---

## **Resources**
- [OpenVPN Documentation](https://openvpn.net/)
- [AWS CloudFormation Guide](https://docs.aws.amazon.com/cloudformation/)
- [Ubuntu Documentation](https://ubuntu.com/server/docs)

---

## **Contributing**
Contributions are welcome! Please fork the repository and submit a pull request with your enhancements or fixes.

---

## **License**
This project is licensed under the MIT License. See the LICENSE file for details.

---

## **Contact**
For questions or support, feel free to reach out via GitHub Issues or email at yousefsamak@yahoo.com.
