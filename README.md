# Setup a Minecraft server with Amazon Elastic Compute Cloud (Amazon EC2)

There are two different types of Minecraft:

- **Java Edition**: support cross-play for Window, Linux, MacOS. The clients must be using Java Edition.
- **Bedrock Edition**: support cross-play for every platforms except Mac/Linux. The clients must be using Bedrock Edition.

For more details about the differences between two types, see [here](https://help.minecraft.net/hc/en-us/articles/360058534412-Differences-Between-Minecraft-Java-Edition-and-Minecraft)

So please ensure that you and your friends are using the same type of Minecraft in order to play together.

**Note**: you will need an AWS account in order to continue. Create one if you don't have one

## Step #1: Setting up a virutal server using Amazon EC2:

Log into your AWS account and head to the EC2 console.

IMAGE HERE

Click the "Lauch Instances" button to go to the setup EC2 instances page

IMAGE HERE

Choose the Amazon Linux 2023 AMI instance from the Amazon Machine Image

IMAGE HERE

There are many options for you to choose from. It depends on how many players you will have in a Minecraft World. For 3-4 players, a t2.small instance will be a good option. So, choose the instance type that meets your needs.

IMAGE HERE

After selecting your instance type, the next step is to set up a key pair for secure server access. You should do this because you don't want your server to be insecurely connected, right? BUT If you continue without key-pair, you wont be able to SSH to your server.

IMAGE HERE

The next step is to setup a Security Group, this is where you allow your server to be accessed. Head to the securiy group page and click "Create security group" on the top right corner to create a security group. In the "Inbound Rules" section, you need to add two rules

- Select **SSH** type and set the **Source** to "Anywhere-IPv4". This will allow you to ssh to your server on your local machine
- Select **Custom TCP** type, set the **Port Range** to **25565** (This is because Minecraft Java Edition communicates over port **25565**) and set the **Source** to "Anywhere-IPv4".

After finish creating the security group, head back to the **Network Settings** section and click "Select existing security group", select the one we've just created and then hit "Lauch instance" to create the instance.
