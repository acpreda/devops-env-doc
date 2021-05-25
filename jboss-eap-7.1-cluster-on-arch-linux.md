# JBoss EAP 7.1 Cluster on Linux Arch

Here are the commands and procedures that you need in order to setup a JBoss EAP 7.1 cluster on a Linux Arch operating system. The SO selection was taken because of the simplicity, performance and memory management.

## Virtual machines creation

The selected cloud provider is AWS because the availability to deploy new instances of Arch Linux. So the first step is to create three virtual machines taking the official AMI from `https://www.uplinklabs.net/projects/arch-linux-on-ec2/`. The code of the AMI we will use is `ami-0002ff9cc5c9b0d98`

Arch Linux image is a minimal installation so it lacks of some utilities packages that must be installed. Follow this commands as the user `arch`

```
sudo pacman -Syu
sudo pacman -S vim unzip
```

 After the update command it's almost sure that the kernel was upgraded so it's a good idea to reboot the machine

```
sudo reboot
```

Access is enabled only for the user `arch` with public key authentication scheme. So, the first thing is to append your public key to the `/home/arch/.ssh/authorized_keys` file on the server, using the `vim` text editor.

```
vim ~/.ssh/authorized_keys
```

Once you are able to access the machine with your own private key then it's time to continue creating the `jboss` user that will be executing the JBoss software. To achieve this follow the commands as user `arch`.

```
sudo useradd -m jboss
```

It's important that you do not assign any password to this user for security reasons.

## JDK 1.8 installation 

You can use either OpenJDK or Oracle JDK. For this case we will use Oracle's JDK 1.8. The JDK's TAR file should be unzipped as `jboss` user.

```
sudo su - jboss
mkdir lib
cd lib
tar xzf ~/jdk-1.8.0_161.tar.gz
ln -s jdk1.8.0_161 jdk
```

Now you have to configure the `JAVA_HOME` environment variable but you want to be permanent, so you have to append the followinf snippet to the `/home/jboss/.bashrc` file using `vim` editor

```
JAVA_HOME="$HOME/lib/jdk"
if [ -d $JAVA_HOME ]; then
	export JAVA_HOME
fi
```

After logging out and logging in again as the `jboss` user you can check that the environment variable `JAVA_HOME` is set correctly.

```
sudo su - jboss
echo $JAVA_HOME
```



 

