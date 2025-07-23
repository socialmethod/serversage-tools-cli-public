# ServerSage CLI Tools
This repository contains command-line tools for managing ServerSage instances. The tools are designed to help with the setup, configuration, and maintenance of ServerSage servers.

## What is ServerSage?
Serversage is a AI agents that can be used to connect you with your severs or computers.
Serversage Agents can translate your natural language requests into commands that can be executed on your servers by leveraging the power of AWS SSM (EC2 Systems Manager) and SSH protocols with the AI Capabilities of modern LLMs.

## Ilustration of ServerSage Architecture

```text
+-----------------------------------------------------------------------------+      +-----------------------------------------------------------------------------+
|                                                                             |      |                                                                             |
|  SERVERSAGE-RESPONSIBILITY                                                  |      |  USER-RESPONSIBILITY                                                        |
|                                                                             |      |                                                                             |
|                                                                             |      |                                                                             |
|                                                                             |      |                                                                             |
|                                                                             |      |                                                                             |
|                                                                             |      |                                                                             |
|      +-----------------------+               +-----------------------+      |      |      +-----------------------+         +-----------------------+            |
|      |                       |               |                       |      |      |      |                       |         |                       |            |
|      |                       |               |                       |      |      |      |                       |         |                       |            |
|      |    serversage-ui      +-------------->|    serversage-api     +------+------+----->|   serversage-bastion  +----+--->|        target-1       |            |
|      |                       |               |                       |      |      |      |                       |    |    |                       |            |
|      |                       |               |                       |      |      |      |                       |    |    |                       |            |
|      +-----------------------+               +-----------+-----------+      |      |      +-----------------------+    |    +-----------------------+            |
|                                                          |                  |      |                                   |                                         |
|                                                          |                  |      |                                   |                                         |
|                                                          |                  |      |                                   |                                         |
|                                                          |                  |      |      +-----------------------+    |    +-----------------------+            |
|                                              +-----------v-----------+      |      |      |                       |    |    |                       |            |
|                                              |                       |      |      |      |                       |    |    |                       |            |
|                                              |                       |      |      |      |     your-local-pc     |    +--->|        target-2       |            |
|                                              |   serversage-agent    |      |      |      |     (your here)       |         |                       |            |
|                                              |                       |      |      |      |                       |         |                       |            |
|                                              |                       |      |      |      +-----------------------+         +-----------------------+            |
|                                              +-----------------------+      |      |                                                                             |
|                                                                             |      |                                                                             |
|                                                                             |      |                                                                             |
|                                                                             |      |                                                                             |
|                                                                             |      |                                                                             |
+-----------------------------------------------------------------------------+      +-----------------------------------------------------------------------------+
```



## Serversage Key Components
### 1. **ServerSage API**
- The API that **ServerSage Agents** live on, the API is used to connect with **ServerSage Bastion**. 
  It provides the necessary endpoints for the **ServerSage Agents** to communicate with the **ServerSage Bastion** and manage the **Target Servers**.

### 2. **ServerSage Agent**
- An AI agent that can translate natural language requests into commands for your **Target Servers**.
- **ServerSage Agents** are designed to understand your requests and execute the appropriate commands on the **Target Servers** via **ServerSage API**.

### 3. **ServerSage Bastion**
- An intermediary server that connects to the **ServerSage API** and manages the communication between the **ServerSage Agent** and your **Target Servers**.
  
- The **ServerSage Bastion** acts as a bridge between the **ServerSage API** and the **Target Servers**, allowing the **ServerSage Agents** to execute commands on the servers.

- **ServerSage Bastion** is shipped as a Docker image that can be run on any server or computer. 
  You can run the **ServerSage Bastion** on your own server or use a cloud provider like AWS, GCP, or Azure to host it.
  
- For connecting the **Serversage API** with the **ServerSage Bastion**, you need to create the **API Key** by adding an environment variable named `API_KEY`. 
  
  **PLEASE NOTE**: 
  - The **ServerSage Bastion** is run by yourself on your own server or computer, and it is not hosted by the ServerSage team. So you need to ensure that the bastion server is running and accessible from the **ServerSage API**. 
  - Please keep in mind that the **ServerSage Bastion** is not a managed service, and you are responsible for its maintenance and availability.

### 4. **Target Servers**
- The servers or computers that you want to manage with **ServerSage Agents**. These can be any servers that you have access to, such as AWS EC2 instances, on-premises servers, or even your local machine.
- **Target Servers** can be Linux or Windows servers, and they can be managed using SSH or AWS SSM (EC2 Systems Manager) protocols.
- **ServerSage Agents** can connect to these servers using SSH or AWS SSM (EC2 Systems Manager) protocols.
- For SSH connections, you need to provide the SSH key and the username of the server.
- For AWS SSM, you need to ensure that the target servers are configured with the necessary IAM roles and permissions to allow the **ServerSage Bastion** to execute commands on them.
- You can connect to your **Target Servers** using SSH by providing the SSH key and the username of the server.

### 5. **AWS SSM (EC2 Systems Manager)**
- AWS SSM is a service that allows you to manage your AWS resources using a secure and scalable way. It provides a way to execute commands on your AWS EC2 instances without the need for SSH access.
- **ServerSage Agents** can leverage AWS SSM to execute commands on your AWS EC2 instances, making it easier to manage your servers without the need for SSH access.

### 6. **SSH**
- SSH (Secure Shell) is a protocol that allows you to securely connect to your servers and execute commands on them.
- **ServerSage Agents** send the commands to the **ServerSage Bastion**, which then executes the commands on the **Target Servers** using SSH.
- You need to provide the SSH key and the username of the server to connect to your **Target Servers** using SSH while. 
- While running the **ServerSage Bastion**, you need to ensure that the SSH key is available on the bastion server.

## Installation
To install the ServerSage CLI tools, you can download the latest release from the [Releases](https://github.com/socialmethod/serversage-tools-cli/releases) page.

Then unzip the downloaded file, then execute the `serversage-tools-cli` binary from the command line.

## Usage
The ServerSage CLI tools provide several commands to manage your ServerSage instances. Here are the explainations of the available features

### Tools Tree Menu
When you run the CLI tools, you will see a menu with the following options:
```text
ubuntu(linux)/
├─ aws ssm protocol/
│  ├─ install aws cli (required)
│  ├─ configure aws cli (required)
│  ├─ create IAM role (required)
│  ├─ setup bastion server/
│  │  ├─ hosted on aws/
│  │  │  ├─ list aws ec2 instances
│  │  │  ├─ run serversage container on bastion
│  │  ├─ hosted on non aws/
│  │  │  ├─ list hybrid activation code
│  │  │  ├─ list servers activated by hybrid activation code
│  │  │  ├─ create hybrid activation code (required)
│  │  │  ├─ activate bastion server with hybrid activation code (required)
│  │  │  ├─ run server as serversage bastion
│  ├─ setup target server/
│  │  ├─ hosted on aws/
│  │  │  ├─ list aws ec2 instances
│  │  │  ├─ run server as serversage target
│  │  ├─ hosted on non aws/
│  │  │  ├─ list hybrid activation code
│  │  │  ├─ list servers activated by hybrid activation code
│  │  │  ├─ create hybrid activation code (required)
│  │  │  ├─ activate target server with hybrid activation code (required)
├─ ssh protocol/
│  ├─ WIP
mac/
├─ WIP
windows/
├─ WIP
```

### Select Your Operating System
When you run the CLI tools, you will be prompted to select your operating system. This is important because the commands and configurations may vary based on the OS you are using.
You can choose from the following options:
1. [**Ubuntu (Linux)**](#linux)
2. [**MacOS**](#macos)
3. [**Windows**](#windows)

### Ubuntu (Linux)
### Select Protocol
After selecting your operating system, you will be prompted to select the protocol you want to use for connecting to your target servers. This protocol will be used by the **ServerSage Bastion** to execute commands on the **Target Servers**.

The available protocols are:
1. [**AWS SSM Protocol**](#1-aws-ssm-protocol)
   
   For connecting to your AWS EC2 instances using AWS Systems Manager (SSM). 
   - This protocol allows you to manage your AWS resources without the need for SSH access.
   - You will also need to create an IAM role with the necessary permissions to allow the **ServerSage Bastion** to execute commands on your AWS EC2 instances using SSM.

2. [**SSH Protocol**](#2-ssh-protocol)

   For connecting to your target servers using SSH.
   This protocol allows you to securely connect to your servers and execute commands on them using SSH.
   - You will need to provide the SSH key and the username of the server to connect to your target servers using SSH.

### 1. AWS SSM Protocol
If you select the AWS SSM protocol, you will have this menu:
1. [**Install AWS CLI (required)**](#11-install-aws-cli-required) 

    This option will install the AWS CLI on your system if it is not already installed. 
    The AWS CLI is required for managing AWS resources and executing commands for setup the **ServerSage Bastion**.
2. [**Configure AWS CLI (required)**](#12-configure-aws-cli-required)

    This option will guide you through the process of configuring the AWS CLI with your AWS credentials. You will need to provide your AWS Access Key ID, Secret Access Key, and the default region you want to use.
3. [**Create IAM Role (required)**](#13-create-iam-role-required)

    This option will create an IAM role with the necessary permissions to allow the **ServerSage Bastion** to execute commands on your AWS EC2 instances using SSM. You will need to provide a name for the IAM role, and the CLI will create the role with the necessary policies attached.
4. [**Setup Bastion Server**](#14-setup-bastion-server)
    
    This option will guide you through the process of setting up the **ServerSage Bastion** server.
5. [**Setup Target Servers**](#15-setup-target-servers)

    This option will guide you through the process of setting up your target servers to allow the **ServerSage Bastion** to execute commands on them using SSM. You will need to ensure that the target servers are configured with the necessary IAM roles and permissions.

### 1.1. Install AWS CLI (required)
The AWS CLI is required for managing AWS resources such as create IAM Role, Create Hybrid Activation Code and other AWS CLI features. This options will ask your sudo password to install the AWS CLI on your system. If you already have the AWS CLI installed, you can skip this step.

### 1.2. Configure AWS CLI (required)
This option will guide you through the process of configuring the AWS CLI with your AWS credentials. You will need to provide your AWS Access Key ID, Secret Access Key, and the default region you want to use. 
- The AWS CLI will store your credentials in the `~/.aws/credentials` file, and the default region in the `~/.aws/config` file.
- If you already have the AWS CLI configured, you can skip this step.

### 1.3. Create IAM Role (required)
This option will create an IAM role with the necessary permissions to allow the **ServerSage Bastion** to execute commands on your AWS EC2 instances using SSM. 
- You will need to provide a name for the IAM role, and the CLI will create the role with the necessary policies attached.
- The IAM role will be created with the following policies:
  - `AmazonSSMManagedInstanceCore`: This policy allows the **ServerSage Bastion** to manage your AWS EC2 instances using SSM.
  - `AmazonEC2RoleforSSM`: This policy allows the **ServerSage Bastion** to execute commands on your AWS EC2 instances using SSM.
- The IAM role will be created in the AWS account you configured in the previous step. 
- If you already have the IAM role created, you can skip this step.

### 1.4. Setup Bastion Server
This option will guide you through the process of setting up the **ServerSage Bastion** server. You will be asked where your bastion will be running, you can choose from the following options:
1. [**Hosted On AWS Server**](#141-hosted-on-aws-server): Hosted on an AWS EC2 instance. This option will guide you through the process of launching an EC2 instance and installing the **ServerSage Bastion** on it.
2. [**Hosted On Non AWS Server**](#142-hosted-on-non-aws-server): Hosted on a non-AWS server or computer. This option will guide you through the process of installing the **ServerSage Bastion** on your own server or computer.

### 1.4.1. Hosted On AWS Server
This option will guide you through the process of launching an EC2 instance and installing the **ServerSage Bastion** on it. This menu have the following options:
- **List AWS EC2 Instances**: This option will list all the AWS EC2 instances in your account.
- **Run ServerSage Bastion**: This option will run the **ServerSage Bastion** server on your AWS EC2 instance. The process will be asking your `instanceId`, `IAM Role`, and `API Key`. Then it will pull the latest **ServerSage Bastion** Docker image from the Docker Hub and run it on your AWS EC2 instance.

### 1.4.2. Hosted On Non AWS Server
This option will guide you through the process of installing the **ServerSage Bastion** on your own server or computer. You will be asked to provide the following information:
- **List Previous Hybrid Activation Codes**: This option will list all the previous hybrid activation codes that you have created.
- **Create Hybrid Activation Code**: This option will create a hybrid activation code that you can use to register your non-AWS server into the AWS Systems Manager (SSM) service.
- **Activate Your Server With Hybrid Activation Code**: This option will guide you through the process of activating your non-AWS server with the hybrid activation code.
- **Run ServerSage Bastion**: This option will run the **ServerSage Bastion** server on your non-AWS server. This process will ask you to select the `Hybrid Activation Code`, `instanceId`, `username`, `SSH Key`, and `API Key`. Then it will pull the latest **ServerSage Bastion** Docker image from the Docker Hub and run it on your non-AWS server.

### 1.5. Setup Target Servers
This option will guide you through the process of setting up your target servers to allow the **ServerSage Bastion** to execute commands on them using SSM. You will be asked where your target hosts are running, you can choose from the following options:
- [**Hosted On AWS Server**](#151-hosted-on-aws-server): Hosted on an AWS EC2 instance. This option will guide you to set up your AWS EC2 instances to allow the **ServerSage Bastion** to execute commands on them using SSM.
- [**Hosted On Non AWS Server**](#152-hosted-on-non-aws-server): Hosted on a non-AWS server or computer. This option will guide you to set up your non-AWS servers or computers to allow the **ServerSage Bastion** to execute commands on them using SSM.

### 1.5.1. Hosted On AWS Server
This option will guide you through the process of setting up your AWS EC2 instances to allow the **ServerSage Bastion** to execute commands on them using SSM. 
- **List AWS EC2 Instances**: This option will list all the AWS EC2 instances in your account.
- **Run ServerSage Target**: This option will makesure the AWS EC2 instances are configured with the necessary IAM roles and permissions to allow the **ServerSage Bastion** to execute commands on them using SSM. This process will ask you to provide the `instanceId` and `IAM Role` of the AWS EC2 instance, then it will makesure the AWS SSM agent is installed and running on the instance, and the necessary IAM role is attached to the instance.

### 1.5.2. Hosted On Non AWS Server
This option will guide you through the process of setting up your non-AWS servers or computers to allow the **ServerSage Bastion** to execute commands on them using SSM.
- **List Previous Hybrid Activation Codes**: This option will list all the previous hybrid activation codes that you have created.
- **List Non AWS Servers Activated**: This option will list all the non-AWS servers or computers that you have registered with the **Hybrid Activation Code**.
- **Create Hybrid Activation Code**: This option will create a hybrid activation code that you can use to register your non-AWS server into the AWS Systems Manager (SSM) service.
- **Run ServerSage Target**: This option will makesure the non-AWS servers or computers are configured with the necessary IAM roles and permissions to allow the **ServerSage Bastion** to execute commands on them using SSM. This process will ask you to provide the `instanceId`, `IAM Role`, and `API Key` of the non-AWS server or computer, then it will makesure the AWS SSM agent is installed and running on the instance, and the necessary IAM role is attached to the instance.

### 2. SSH Protocol
WIP: The SSH protocol is still a work in progress, and the CLI tools will be updated to support it in the future.
<!-- If you select the SSH protocol, you will have this menu:
- **Run ServerSage Bastion**: This option will run the **ServerSage Bastion** server on your bastion server. The bastion server will be used to connect to your target servers using SSH. This process will ask you to provide the IP address of your bastion server, the SSH key, and the username of the server. -->

<!-- ### 2.1. Run ServerSage Bastion
This option will run the **ServerSage Bastion** server on your bastion server.
- The process will ask you to provide the following information:
  - **Bastion Server IP Address**: The IP address of your bastion server
  - **SSH Key**: The SSH key that will be used to connect to your bastion server
  - **Username**: The username of the bastion server
  - **API Key**: The API key that will be used to connect to the **ServerSage API**.
- After providing the necessary information, the CLI will pull the latest **ServerSage Bastion** Docker image from the Docker Hub and run it on your bastion server.
- The **ServerSage Bastion** server will be running on the bastion server, and it will be used to connect to your target servers using SSH.

### Setup Bastion Server
This option will guide you through the process of setting up the **ServerSage Bastion** server. You will asked where your bastion will running, you can choose from the following options:
- **Hosted On AWS Server**: Hosted on an AWS EC2 instance. This option will guide you through the process of launching an EC2 instance and installing the **ServerSage Bastion** on it.
- **Hosted On Non AWS Server**: Hosted on a non-AWS server or computer. This option will guide you through the process of installing the **ServerSage Bastion** on your own server or computer.

### 5. Hosted On AWS Server -->

### MacOS
WIP: The MacOS support is still a work in progress, and the CLI tools will be updated to support it in the future.
### Windows
WIP: The Windows support is still a work in progress, and the CLI tools will be updated to support it in the future.

  


