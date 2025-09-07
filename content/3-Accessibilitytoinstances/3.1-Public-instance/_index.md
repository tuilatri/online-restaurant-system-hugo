---
title : "Connect, Install, and Deploy"
date : "2025-09-06" 
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---

### Detailed Guide to Commands on MobaXterm

In this section, you will execute commands to connect to the `ors-ec2` server, then install the environment and deploy the Node.js backend application.

{{% notice info %}}
**Note:** The commands must be executed sequentially. Ensure you have completed the previous step before moving on to the next.
{{% /notice %}}

#### **1. Connect to the EC2 Instance**
This is the first step, connecting from your computer to the `ors-ec2` server in the Public Subnet.

*   Open MobaXterm and create a new **SSH** session.
*   **Remote host:** Enter the **Public IPv4** address of `ors-ec2` (you can get this from the EC2 Dashboard).
*   **Specify username:** `ec2-user`.
*   **Use private key:** Select the `ors-keypair.pem` file you downloaded.
*   **Set permissions for the key file (if using Terminal on macOS/Linux):**
    Before connecting, you need to run this command on your personal computer to secure the key file:
    ```bash
    chmod 400 ors-keypair.pem
    ```

{{< figure src="/images/3.connect/3.1-deploy/mobaxterm-connect-01.png" title="Setting up the SSH session in MobaXterm" >}}

#### **2. Install the Environment on the Server**
Once successfully connected to the server, install the necessary tools.

*   **Update the system:**
    ```bash
    sudo yum update -y
    ```
    {{< figure src="/images/3.connect/3.1-deploy/yum-update-01.png" title="Updating system packages" >}}

*   **Install Git:**
    ```bash
    sudo yum install -y git
    ```

*   **Clone the project source code from GitHub:**
    ```bash
    git clone https://github.com/tuilatri/online-restaurant-system.git
    ```
    {{< figure src="/images/3.connect/3.1-deploy/git-clone-01.png" title="Cloning the project" >}}

*   **Move into the backend directory:**
    ```bash
    cd online-restaurant-system/backend
    ```

*   **Install Node.js and npm:**
    ```bash
    sudo dnf install -y nodejs
    ```
    {{< figure src="/images/3.connect/3.1-deploy/install-nodejs-01.png" title="Installing Node.js" >}}

*   **Check the versions to confirm successful installation:**
    ```bash
    node -v
    npm -v
    ```

#### **3. Install and Configure the Application**

*   **Install the project's dependencies:**
    ```bash
    npm install
    ```
    {{< figure src="/images/3.connect/3.1-deploy/npm-install-01.png" title="Installing npm packages" >}}

*   **Move into the `src` directory and create the environment variable configuration file:**
    ```bash
    cd src
    nano .env
    ```

*   **Enter the database connection information:**
    In the `nano` editor, paste the following content and replace it with your information.
    ```env
    DB_HOST='your-rds-endpoint'
    DB_USER='admin'
    DB_PASSWORD='your-db-password'
    DB_DATABASE='ors'
    DB_PORT=3306
    ```
    {{< figure src="/images/3.connect/3.1-deploy/nano-env-01-01.png" >}}
    {{< figure src="/images/3.connect/3.1-deploy/nano-env-02-01.png" title="Configuring the .env file" >}}

*   **Save and exit `nano`:**
    1.  Press `Ctrl + O`
    2.  Press `Enter` to confirm the file name.
    3.  Press `Ctrl + X` to exit.

#### **4. Launch and Test the Backend**

*   **Launch the application:**
    ```bash
    node createAdmin.js
    node server.js
    ```
    {{< figure src="/images/3.connect/3.1-deploy/npm-start-01.png" title="Backend application running on port 5000" >}}

*   **Test the application's operation directly on the server:**
    Open a **new** terminal window in MobaXterm and connect to the same EC2 instance again, then use the `curl` command to test.
    ```bash
    curl http://127.0.0.1:5000/api
    ```
    If you receive the response `Welcome to Dining Verse Backend API!`, it means the application has started successfully!

#### **5. Troubleshooting Guide**
If you update the code on GitHub but the changes do not appear on the web, follow these steps on the server:

*   **Move into the backend directory:**
    ```bash
    cd online-restaurant-system/backend
    ```

*   **Pull the latest code:**
    ```bash
    git pull origin main
    ```
*   **Find the running Node.js process:**
    ```bash
    ps aux | grep node
    ```
*   **Stop the old process using its PID:**
    ```bash
    kill -9 <PID>
    ```
    *Practical example:*
    ```
    [ec2-user@ip-10-0-144-101 backend]$ ps aux | grep node
    ec2-user    3117  0.0  6.6 1127836 64896 pts/1   Sl+   10:15   0:00 node server.js
    
    [ec2-user@ip-10-0-144-101 backend]$ kill -9 3117
    ```

*   **Restart the application:**
    ```bash
    npm run start
    ```