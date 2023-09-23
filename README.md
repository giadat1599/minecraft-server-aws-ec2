# Setup a Minecraft server with Amazon Elastic Compute Cloud (Amazon EC2)

There are two different types of Minecraft:

- **Java Edition**: Support cross-play for Windows, Linux, and MacOS. The clients must be using the Java Edition.
- **Bedrock Edition**: Support cross-play for every platform except Mac/Linux. The clients must be using the Bedrock Edition.

For more details about the differences between two types, see [here](https://help.minecraft.net/hc/en-us/articles/360058534412-Differences-Between-Minecraft-Java-Edition-and-Minecraft).

So please ensure that you and your friends are using the same type of Minecraft in order to play together.

**Note**: You will need an AWS account to continue. If you don't have one, please create an account.

## Step #1: Setting up a virutal server using Amazon EC2

Log into your AWS account and head to the EC2 console. Click the "Launch Instances" button to go to the EC2 instances creation page.

<img width="1750" alt="Screenshot 2023-09-22 at 18 15 02" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/3478083d-1e69-40a0-8fa9-e094632f6844">


Enter your server name and choose the Amazon Linux 2023 AMI instance from the Amazon Machine Image

<img width="1750" height="550" alt="Screenshot 2023-09-22 at 18 17 03" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/7b9e9aea-89f1-424a-820b-93ae3aeead21">


The next step is to choose your instance type; there are many options for you to choose from. It depends on how many players you will have in a Minecraft world. For 3-4 players, a t2.small instance will be a good option. Therefore, choose the instance type that meets your needs.


<img width="1788" alt="Screenshot 2023-09-22 at 17 28 01" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/e9940a71-39fe-4194-aa39-e11bb75dfbca">


After selecting your instance type, the next step is to set up a key pair for secure server access. You should do this because you don't want your server to be insecurely connected, right? BUT If you continue without key-pair, you wont be able to SSH to your server. So, create and download the key pair, store it in a folder.

<img width="1788" height="550"  alt="Screenshot 2023-09-22 at 17 40 01" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/e6d203de-b66d-49d0-85ce-013dd4c61dff">


The next step is to setup a Security Group, this is where you allow your server to be connected. Head to the securiy group page and click "Create security group" on the top right corner to create one. In the "Inbound Rules" section, you need to add two rules:

- Add **SSH** type and set the **Source** to "Anywhere-IPv4". This will allow you to ssh to your server on your local machine
- Add **Custom TCP** type, set the **Port Range** to **25565** (This is because Minecraft Java Edition communicates over port **25565**) and set the **Source** to "Anywhere-IPv4".

After creating the security group, head back to the **Network Settings** section (in the EC2 creation page) and click "Select existing security group", select the one we've just created and then hit "Launch instance" to create the instance.

<img width="1511" alt="Screenshot 2023-09-22 at 18 25 36" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/6bfc28d0-a983-4bb5-92b8-31491e7abc9b">


&nbsp;

<img width="1675" alt="Screenshot 2023-09-22 at 17 45 14" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/461a827f-cd09-47e2-ab13-478fce622dd2">

&nbsp;

<img width="1234" alt="Screenshot 2023-09-22 at 18 10 58" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/98837b84-9f3f-4a3b-9693-9a928252511e">

## Step #2: Connect to your server

First of all, you need to connect to your server. There are 2 ways of connecting to your server: use "EC2 Instane Connect" in AWS or connect from your local machine via SSH. I will use SSH to continute.

Open your computer terminal in the folder where you've stored the `.pem` file (This is the key pair we downloaded in the previous step).

<img width="1787" alt="Screenshot 2023-09-23 at 21 52 44" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/4dc336ce-c696-4758-852a-4ded4d9fc01c">

Enter the commands below.

-  `chmod 400 minecraft_ssh.pem`: this will ensure your key is not publicly viewable (Run this command, if necessary).
-  `ssh -i minecraft_ssh.pem ec2-user@<YOUR INSTANCE PUBLIC IPV4>`: this will establish a connection to you server with the root user `ec2-user` (this is the default user when you create ec2 instance with amazon linux) and your ec2 instance public ipv4 (Get this ipv4 in the instance detail)

<img width="1501" alt="Screenshot 2023-09-23 at 22 08 22" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/ed3bf15e-02ed-412e-931c-c4202a2650e9">

&nbsp;

After executing those two commands, you will be able to connect to your server.

<img width="1783" alt="Screenshot 2023-09-23 at 22 11 16" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/2a2fac0d-9ad4-4cf5-af49-d650b01bb107">

## Step #3: Install Java on the server
We will use Amazon Corretto to get Java 17 installed, which will run Minecraft. You can follow [Corretto 17 User Guide](https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/amazon-linux-install.html) or just simply type the command below.

`sudo yum install java-17-amazon-corretto`

After executing the command, it will download and install Java 17 on your server. You can verify this by typing `java --version`

<img width="1789" alt="Screenshot 2023-09-23 at 22 21 36" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/710ad70b-ac5c-409e-a893-40ef44b2d0fe">

## Step #4: Install Minecraft
Create folder to store the minecraft server.

- `sudo su` to switch to the root user

- `mkdir minecraft_server` to create a folder called `minecraft_server`

- `cd minecraft_server` jump into the folder

![image](https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/3a314d8e-6ee3-47e7-818a-43a72cddf96b)

Download the minecraft server file. See [here](https://www.minecraft.net/en-us/download/server) or enter the command bellow.

`wget https://piston-data.mojang.com/v1/objects/5b868151bd02b41319f54c8d4061b8cae84e665c/server.jar`

<img width="1788" alt="Screenshot 2023-09-23 at 22 30 59" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/458e86e5-5d75-4d42-964b-9269c9bd27fc">

## Step #5: Running the minecraft server

Run this command  `java -Xmx1024M -Xms1024M -jar server.jar nogui`

After running the command, you probably ran into this error.

<img width="1789" alt="Screenshot 2023-09-23 at 22 35 18" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/b468e446-8e1a-4313-a358-ea9ed6b5662b">

To fix this, we need to change `eula=false` to `eula=true` in the `eula.txt`. I will use `vi` editor to edit the file or any editor you want:

`vi eula.txt`

Hit `i` to  enter insert mode, move to the line of `eula` and change to `eula=true`

<img width="1790" alt="Screenshot 2023-09-23 at 22 39 03" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/0cd58ad8-f31d-4034-b6b6-693763d1a854">

Press ESC to exit insert mode and type `:wq` to save and quit

Run this command again  `java -Xmx1024M -Xms1024M -jar server.jar nogui` and wait


<img width="1790" alt="Screenshot 2023-09-23 at 22 44 57" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/aa17e80e-16e3-4d3d-8f2e-755bb0b94f3b">

If you see "DONE" which means your minecraft server has been started succesfully and now you are able to connect to your minecraft server with your friends using the public IPv4 of the server.

## Final step: Get your friends and join the server

Open your minecraft and type the server name and the IP.

<img width="1165" alt="Screenshot 2023-09-23 at 23 03 01" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/8f2d172c-ef32-40dc-9329-7207a2ed80f3">

<img width="1588" alt="Screenshot 2023-09-23 at 23 03 57" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/bb0dae61-d005-4a58-8dc7-82a6543165a0">

<img width="1509" alt="Screenshot 2023-09-23 at 23 04 17" src="https://github.com/giadat1599/minecraft-server-aws-ec2/assets/114280300/4bd9cb49-4311-4e64-8dd9-9505578531a5">

For game configuration, read the configuration in `server.properties`. See [here](https://codakid.com/minecraft-server-properties/)
 










