## How to set up a Jenkins server using AWS - change

Note - Please refer to Jenkins documentation for updates to this process:
https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/

## Creating the Jenkins server on AWS

Login in to AWS.

Create a security group for Jenkins servers. It needs:

Inbound - 
SSH -> Your IP
HTTP -> 0.0.0.0
Port 8080 -> 0.0.0.0

Now make a new instance and use the SG we created (note there are AMI's that already have Jenkins ready if you want to use them)

SSH in using your usual key pair.


## Downloading and installing Jenkins on the EC2 server

Enter the instnace via SSH

1. Run `sudo apt update` and then `sudo apt-get upgrade`
2. Add the Jenkins repo key so linux know where to go when asked:
```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
``` 
3. Next add the url to the sources.list of ubuntu :
```
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```
4. Run an update: `sudo apt update`
5. You can check the versions of Jenkins available with:
```
sudo apt-cache madison jenkins
```
7. Check Java is installed properly:
```
java -version
```

8. Then install jenkins with:
```
# To install latest version
sudo apt install jenkins

# To install specific version
sudo apt-get install jenkins=2.138.3 -y
```
Note, if you install an old version of Jenkins, you need to check what version of Java it needs

9. Allow Jenkins to run at boot (it needs to do this to actually work):
```
sudo systemctl enable jenkins
```
10. Start the Jenkins server:
```
sudo systemctl start jenkins
```
11. Check Jenkins is now running:
```
sudo systemctl status jenkins
```

## Setting up Jenkins
Jenkins should now be available in your web bowser. Go to the public i.p of the EC2 instance and add:8080 (make sure you are using http and https)

You will get this page:

![Alt text](/images/jenkins-1.jpg "Initial Jenkins screen")

On this page you need to get the temporary admin password from the EC2 instance so you can log in, and then change it. 

To find this password, enter the follwoing command into your EC2 instance terminal:
```
# It will print the contents of the file using sudo cat
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy and paste this into the password section of the webpage.

Next you will get this page, here you can specify any plugins you want too install. But for now just choose suggested. We can always add more later.

![Alt text](/images/jenkins-2.jpg "Plugins screen")

Now Jenkins will being to install everything it needs:

![Alt text](/images/jenkins-3.jpg "Installation process")

At this point you will be prompted to create your first admin user:

Just follow the steps.

![Alt text](/images/jenkins-4.jpg "First admin user")

Confirm the default Jenkins url (just keep it the same):

![Alt text](/images/jenkins-5.jpg "Default Jenkins URL")

Jenkins is now ready to use!

![Alt text](/images/jenkins-6.jpg "Default Jenkins URL")

## Configuring Jenkins (with AWS)

Go to "Manage Jenkins" on the left hand side of the page. Then choose "Manage plugins".

Now move to the "Available" tab, and use the search bar to find the EC2 plugin:

When you go to install it, make sure to use "Install without restart".

![Alt text](/images/jenkins-7.jpg "Installing the EC2 plugin")

After successful installation:

![Alt text](/images/jenkins-8.jpg "Plugin installed")

Go back to the Dashboard, find the "Configure a cloud" option:

![Alt text](/images/jenkins-9.jpg "Configure a cloud option")

Click "Manage Nodes" on the left of the page. Then "Configure Clouds".

On this page, select the dropdown and choose "Amazon EC2".

In the setup page that you are given, add Jenkins for AWS credentials. Here you will need to add your AWS access key and Secret key. Please do not share this with anyone!

![Alt text](/images/jenkins-10.jpg "Adding credentials to Jenkins")

Save this, select the Access key from the drop down and scroll down the page.

Select eu-west-1 as your region and click "Add" to add your ssh key pair for AWS:

![Alt text](/images/jenkins-11.jpg "Adding credentials to Jenkins 2")

Once this is open, choose "SSH username with private key"

Make sure to use the correct username. In most cases this is "ubuntu". Check what it is in your EC2 terminal.

You now have to enter the key directly (copy and paste it). Again MAKE SURE NOT TO SHARE THIS WITH ANYONE!

Now test the connecction to make sure Jenkins can get into AWS:

![Alt text](/images/jenkins-12.jpg "Checking AWS connection")

If it says "success" then you can click "Save". And you are done!










