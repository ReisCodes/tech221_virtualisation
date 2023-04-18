# Virtualisation

### What is virtualisation?

Virtualization is the technology that creates virtual versions of various resources such as the operating system, server, storage etc. thus allowing software to replicate hardware functionality and resulting in the creation of a virtual system independent from the hardware.

### What is a Dev Environment? and what is its purpose?

A Dev Environment is a workspace where developers can run, test and deploy certain applications in a closed environment, you can think of this as a test environment. 

The purpose of this is so developers can test certain things before pushing it to actual live environment where these new features may have issues.

### How to create a VM using vagrant & VirtualBox

1. First step is to make sure vagrant and virtualbox are both downloaded and installed onto your local machine. 

2. Create a Folder where you want to store your VM. 

3. Within VSCode open this folder and in the terminal run, `vagrant init` will initialise the vm on your loacl machine 

4. You will have a file created within your folder named "Vagrant file", this file needs to ammended to look like: 

![](image1..png)

5. You can now go back to the terminal and run `vagrant up`. To make sure our VM has been created move to our VirtualBox tab and our VM should be "Running".

![](image3.png)

6. Moving to our terminal/bash window if we use the command `vagrant ssh`, we use this to access the vm. we should recieve a welcome message as so:

![](image2.png)

### Installing Nginx on our VM using provisioning

1. The first step we need to do is in our Vagrantfile, we need to add a configeration step to allow provision with its path:

![](image4.png)
 
2. We now need to write our script file in the same directory, called "provision.sh":

```
!#/bin/bash

sudo apt-get -y update 

sudo apt-get -y upgrade 

sudo apt install nginx -y 

service nginx start
```

3. We are now ready to `vagrant up`, we can check our vm is up and running with nginx installed by copying the IP into our browser where we should be greeted with the nginx home screen, or using `service nginx status` and returned with a green "active (running)" message.

![](image5.png)

### Syncing folders into our VM

1. We first need to add our files to the same directory of our vagrant file.

![](image6.png)

2. In our "Vagrantfile" we now need to add a configeration step to add this file to our VM directory. This tells the vm what folder to sync and the path we want to store it at.

`config.vm.synced_folder "app", "/home/vagrant/app"`

3. We can now `vagrant run`, once the VM has been created we can `cd app` to check if this file exists within our vm and `ls` to check the contents.

![](image7.png)

# Deploying an application on a VM

### Without provisioning:

1. Our VM that is on ubuntu OS will have python installed but we need to add  a python package manager, we can do this by opening a terminal-bash window, ssh into our vm and use the cmd:

- `sudo apt-get install python-software-properties`

2. Now to install node on a Linux system we have to use 2 commands to achieve this:

- `curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -`

Once this is done, your window will display a message prompting you to input a new cmd, it should look something like: 

Run `sudo apt-get install -y nodejs` to install Node.js 6.x

*You can check your version of node.js by using `nodejs --version`*

3. We now want to install "PM2" this is an advanced process manager for running node.js apps, to do this we use:

- `sudo npm install pm2 -g`

4. Once that has completed we need to cd into our app folder

- `cd app`

5. Our final steps are to install npm within our application and starting it, we can do this with :

- `npm install`

and then:

- `npm start`

You should be greeted with this message.

![](image8.png)

6. You can now copy your IP address and add your port on the end to direct you to your deployed application, in this case our code is: "http://192.168.10.100:3000/"

![](image9.png)

### With Provisioning:

1. Provisioning is the process of setting up the Vagrant box when it is ran for the first time. Provisioning will only run once for a new VM, unless forced.

This one specifically is executing a bunch of shell commands when it is being provisioned.

`config.vm.provision :shell do |shell|`

2. Within our "Vagrant" file we need to set up a provision to do all the instilations we need for the vm, we can do this with:

![](image10.png)

3. We then need to set up the provisions for the npm to start within the application directory. 

![](image11.png)

4. Now when we `vagrant up`, our vm should automatically deploy our application and return the same message and port number. we can return to this page and see if our application has been deployed. ("http://192.168.10.100:3000/")

![](image9.png)
