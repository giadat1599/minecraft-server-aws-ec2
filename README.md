# Setup a Minecraft server with Amazon Elastic Compute Cloud (Amazon EC2)

There are two different types of Minecraft:

- **Java Edition**: support cross-play for Window, Linux, MacOS. The clients must be using Java Edition.
- **Bedrock Edition**: support cross-play for every platforms except Mac/Linux. The clients must be using Bedrock Edition.

For more details about the differences between two types, see [here](https://help.minecraft.net/hc/en-us/articles/360058534412-Differences-Between-Minecraft-Java-Edition-and-Minecraft)

So please ensure that you and your friends are using the same type of Minecraft in order to play together.

**Note**: you will need an AWS account in order to continue. Create one if you don't have one

## Step #1: Setting up a virutal server using Amazon EC2:

Log into your AWS account and head to the EC2 console. Click the "Lauch Instances" button to go to the setup EC2 instances page.

<img width="1788" alt="Screenshot 2023-09-22 at 17 20 06" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/bf89c6a7-deae-4827-90fd-9bc4e09adcc4">


Enter your server name and choose the Amazon Linux 2023 AMI instance from the Amazon Machine Image

<img width="1788" height="550" alt="Screenshot 2023-09-22 at 17 24 11" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/09e42054-23d4-4dc8-8ffd-9a6b048cd90e">


The next step is to choose your instance type, there are many options for you to choose from. It depends on how many players you will have in a Minecraft World. For 3-4 players, a t2.small instance will be a good option. So, choose the instance type that meets your needs.


<img width="1788" alt="Screenshot 2023-09-22 at 17 28 01" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/e9940a71-39fe-4194-aa39-e11bb75dfbca">


After selecting your instance type, the next step is to set up a key pair for secure server access. You should do this because you don't want your server to be insecurely connected, right? BUT If you continue without key-pair, you wont be able to SSH to your server.

<img width="1788" height="550"  alt="Screenshot 2023-09-22 at 17 40 01" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/e6d203de-b66d-49d0-85ce-013dd4c61dff">



The next step is to setup a Security Group, this is where you allow your server to be accessed. Head to the securiy group page and click "Create security group" on the top right corner to create a security group. In the "Inbound Rules" section, you need to add two rules

- Select **SSH** type and set the **Source** to "Anywhere-IPv4". This will allow you to ssh to your server on your local machine
- Select **Custom TCP** type, set the **Port Range** to **25565** (This is because Minecraft Java Edition communicates over port **25565**) and set the **Source** to "Anywhere-IPv4".

After finishing creating the security group, head back to the **Network Settings** section (in the EC2 creation page) and click "Select existing security group", select the one we've just created and then hit "Lauch instance" to create the instance.

![image](https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/c414e60e-4136-4e45-b0e5-f7b608bfef0b)

<img width="1675" alt="Screenshot 2023-09-22 at 17 45 14" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/461a827f-cd09-47e2-ab13-478fce622dd2">

<img width="1234" alt="Screenshot 2023-09-22 at 18 10 58" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/98837b84-9f3f-4a3b-9693-9a928252511e">
