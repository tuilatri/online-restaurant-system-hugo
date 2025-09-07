---
title : "Connecting to the EC2 Instance"
date : "2025-09-04" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

After building the entire infrastructure on AWS, the next step is to connect to the virtual servers (EC2 instances) to deploy the backend application's source code.

Due to our architecture having Public and Private Subnets, we cannot connect directly to the Backend server from the Internet. Instead, we will follow a secure connection process:
1.  Connect from your personal computer to the **Bastion Host** (located in the Public Subnet).
2.  From the Bastion Host, continue to connect to the **Backend Instance** (located in the Private Subnet).

{{% notice info %}}
In this chapter, **MobaXterm** will be used as the SSH client on Windows to perform the connections. You can also use other tools such as Terminal (macOS/Linux), PuTTY, or Windows Terminal.
{{% /notice %}}