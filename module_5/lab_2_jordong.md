# A view of the route tables for your VPC in Task 1
![](./pics/Screenshot%202023-08-29%20at%2011.32.03%20PM.png)

# A view showing the subnet created in Task 2
![](./pics/Screenshot%202023-08-29%20at%2011.35.36%20PM.png)

# A view of your security group in-bound rules of Task 3
![](./pics/Screenshot%202023-08-29%20at%2011.41.01%20PM.png)

# Your public web page from the end of Task 4
![](./pics/Screenshot%202023-08-29%20at%2011.52.30%20PM.png)

A virtual private cloud offers users a private space to launch resources. It provides enhanced security for network architecture that mirrors on-premises setups. 

Subnets divide a larger IP network enhancing performance, security, and efficiency. Private subnets house resources that can't be accessed from the internet using a NAT gateway. Public subnets contain resources that can be accessed from the internet using an Elastic or Public IP. You would use private subnets for sensitive objects like databases, and public subnets for things like web servers.

To make a web page you have to have a public subnet made in your VPC. Then you can launch an instance within that subnet. Then you make sure it has an Elastic IP and configure the security group. Then you install a web server like Apache and deploy your page. 