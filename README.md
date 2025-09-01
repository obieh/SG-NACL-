# Security Group-Network Access Control List project

## This mini project will explore the core concept of AWS Security Group (SG) and Network Acess Control List (NACL).

### Security Group (SG): Acts as a stateful firewall at the instance level (e.g., for a single EC2 instance).

### Network ACL (NACL): Acts as a stateless firewall at the subnet level (for an entire group of instances).

## Security Group (SG)
* Operates at the instance level. It's the first line of defense for an EC2 instance, Lambda function, or RDS database.

* Stateful: This is the key characteristic. If you send a request outbound from your instance (e.g., to download a software update), the response traffic is automatically allowed to flow back in, regardless of the inbound rules. The return traffic is not evaluated against the inbound rules.

* Rules:
    - ALLOW rules only. You cannot create explicit "DENY" rules. The default behavior is to deny all inbound traffic and allow all outbound traffic until you add your own rules.
    - Rules are based on protocol, port, and source/destination IP (or another Security Group).
* Evaluation: All rules are evaluated before deciding whether to allow traffic.

### Analogy: A bouncer at a specific club's door (the EC2 instance) who checks everyone coming in but remembers who went out so they can get back in.

## Network Access Control List (NACL)

* Operates at the subnet level. It is an optional second layer of defense that controls traffic in and out of an entire subnet.

* Stateless: Responses to allowed inbound traffic are not automatically allowed to return. You must explicitly create outbound rules for the return traffic. The same is true for initiated outbound traffic—you need a corresponding inbound rule for the response.

* Rules:
   - ALLOW and DENY rules. You can explicitly block specific IP addresses or ports.

   - Rules are processed in numerical order (lowest to highest) until a matching rule is found. This is crucial for its operation.

   - The default NACL that comes with a VPC allows all inbound and outbound traffic
* Evaluation: Rules are processed in order, and the first match applies.

### Analogy: A firewall at the entrance to a whole neighborhood (the subnet) that checks all cars going in and out. It doesn't remember any car; every trip is checked against the rules.

### Key Differences Table

| Feature | Security Group (SG)| Network ACL (NACL) |
| :------- |------------------- | ------------------ |
| Scope   | Instance level (applies to ENI) | Subnet level (applies to all instances in the subnet) |
| State   | Stateful (Return traffic is automatically allowed) | Stateless (Return traffic must be explicitly allowed by rules) |
| Rule Types | Allow rules only (implicit deny) | Allow AND Deny rules (explicit) |
| Rule Order | All rules are evaluated; no order needed. | Evaluated in numerical order (e.g., Rule 100 before Rule 200). |
| Default Behavior | Deny all inbound; Allow all outbound | Default NACL: Allow all inbound and outbound. Custom NACL: Deny all inbound and outbound. |
  

## Create an Instance in public subnet

* head over to EC2 page, add a name and select os image you want to use.

![](./img/Pasted%20image%20(2).png)

* Select the vpc you have created in the previous project.

![](./img/Pasted%20image.png)

* Add a security group to allow ssh and all ipv4

![](./img/Pasted%20image%20(3).png)

* The instance should come up running. Head over to instnaces running and select the instace you just created.

![](./img/Pasted%20image%20(4).png)

* Click security group to see the details. Inbound rules should allow ssh traffic on port 22.

![](./img/Pasted%20image%20(5).png)

* Outbound rules should allow all IPv4 traffic.

![](./img/Pasted%20image%20(6).png)

* Now copy the public IP address and headover to your browser to access the app. You will notice that the connection times out.

![](./img/Pasted%20image%20(7).png)