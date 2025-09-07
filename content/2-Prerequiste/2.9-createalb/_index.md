---
title : "Create Application Load Balancer (ALB)"
date : "2025-09-04"
weight : 8
chapter : false
pre : " <b> 2.8 </b> "
---

### Initialize Application Load Balancer (ALB)

ℹ️ **Objective**

*   The **Application Load Balancer (ALB)** acts as the entry point for receiving and distributing user traffic to the backend servers.
*   By placing the ALB in Public Subnets across multiple Availability Zones (AZs), we ensure the application has high availability. If one AZ experiences an issue, the ALB will automatically redirect traffic to the servers in the remaining AZs.
*   The ALB will listen for incoming requests and forward them to the **Target Group** (`project-backend-target-group`) that we created in the previous step.

---

🔒 **Execution Steps**

#### **1. Start Creating the Load Balancer**

*   From the **EC2 Dashboard**, in the left menu, scroll down to the **Load Balancing** section and select **Load Balancers**.
*   Click on **Create load balancer**.

{{< figure src="/images/2.prerequisite/2.8-createalb/create-lb-button.png" title="Starting to create a Load Balancer" >}}

#### **2. Choose the Load Balancer Type**

*   There are several types of Load Balancers. Since our application operates at the application layer (HTTP/HTTPS), select **Application Load Balancer** by clicking **Create**.

{{< figure src="/images/2.prerequisite/2.8-createalb/choose-alb-type.png" title="Choosing Application Load Balancer" >}}

#### **3. Configure Basic Information**

*   **Load balancer name:** `project-backend-alb`
*   **Scheme:** `Internet-facing` (Because this ALB will receive traffic directly from the internet).
*   **IP address type:** `IPv4`

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-basic-config.png" title="Filling in the basic configuration for the ALB" >}}

#### **4. Configure Network Mapping**

*   This is a crucial step to ensure the ALB is accessible from the internet.
*   **VPC:** Select `project-vpc`.
*   **Mappings:** Select both Availability Zones we are using. For each AZ, choose the corresponding **Public Subnet**.

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-network-mapping.png" title="Selecting the VPC and Public Subnets" >}}

#### **5. Configure Security Groups**

*   Keep the `default` Security Group selected.
*   From the dropdown list, select the Security Group created specifically for the ALB: `project-alb-sg`.

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-security-group.png" title="Selecting the Security Group for the ALB" >}}

#### **6. Configure Listeners and Routing**

*   This is where we define how the ALB handles incoming requests.
*   **Listener:** Keep `HTTP` and Port `80` as they are.
*   **Default action:** From the dropdown list, select the `project-backend-target-group` created in the previous step. This action instructs the ALB to forward all traffic from port 80 to this Target Group.

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-listeners-routing.png" title="Configuring the Listener and forwarding to the Target Group" >}}

#### **7. Finalize, Check Status, and Test Access**

*   Review the information in the **Summary** section and click **Create load balancer**.
*   After it's created successfully, click **View load balancer**.

{{% notice info %}}
**Note:** The process of provisioning a Load Balancer will take about 5-10 minutes. Its state will change from `provisioning` to `active`. Please be patient.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-status-provisioning.png" title="Waiting for the ALB to become Active" >}}

*   Once the ALB is `active`, select it and copy the **DNS name** from the **Details** tab.

{{< figure src="/images/2.prerequisite/2.8-createalb/alb-copy-dns.png" title="Copying the ALB's DNS name" >}}

*   You will be able to see the result after you connect to the Backend and deploy the project in a later step.
*   Paste this DNS name into a browser. If everything is configured correctly, you will see the message: `Backend is running!`