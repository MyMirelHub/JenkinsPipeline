# Implementing a full CI/CD pipeline

### Installing git

git will be used as the main SCM 

```
sudo apt install git
```
### Configuring name and email

Set the name and email asscociated to you github account
```
git config --global user.name “mirelCD1”

git config --global mirel.house@gmail.com
```

### Private Key Access

Add a new ssh key to github account

*Reference* 

https://help.github.com/en/enterprise/2.15/user/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account


#### Generate a new SSH Key

```bash
ssh-keygen -t rsa -b 4096 -C "mirel.house@gmail.com"
```

```bash
#retrieve the agent id
eval $(ssh-agent -s)

#Add the key to the SSH agent
 ssh-add ~/.ssh/id_rsa
```

#### Add they key to the github account

Download and install xclip, and once copied to the cliboard to go to GitHub settings and add the key. 

``` bash
sudo apt-get install xclip
# Downloads and installs xclip. If you don't have `apt-get`, you might need to use another installer (like `yum`)

xclip -sel clip < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your clipboard
```

### Build Automation (Gradle)
Build automation is the automation of tasks needed in order to process and prepare source code for
deployment to production.

Install Gradle

```bash
sudo apt install gradle

#check gradle version
gradle --version
```

#### Installing the gradle wrapper
Gradle Wrapper allows Gradle to install itself using just the files from your project’s source control.

The goal is to connect the SCM to Jenkins and gradle is the wrapper which handles the link between them.

First create project dir 
```
mkdir gradle-projects/frist-gradle-project
```
Within the project dir, create a grade proejct and build a skeleton

```
gradle wrapper

./gradlewbuild
```

Install Jenkins

https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-18-04

  Start Jenkins

```bash 
sudo systemctl start jenkins

#Check Status
sudo systemctl status jenkins
```

Configure the firewall for Jenkins

```bash
#Activate firewall
sudo ufw enable

#Allow port 8080
sudo ufw allow 8080

#Check the status
sudo ufw status
```

The status should show that port 8080 is active from anywhere

```
Status: active

To                         Action      From
--                         ------      ----
8080                       ALLOW       Anywhere                  
8080 (v6)                  ALLOW       Anywhere (v6) 
```

Access Jenkins

If running from a your local server access the admin password using

```bash
sudo nano /var/lib/jenkins/secrets/initialAdminPassword
```

```bash
# Replace localhost with your IP
localhost:8080
```
Install suggested plugins and follow prompts, 

- Create a feestyle project
- Copy the train schedule github repo and put it into Github project

https://github.com/linuxacademy/cicd-pipeline-train-schedule-jenkins
- Paste it in ```Git``` under source code management

This is only reading from a public repository so do not need credentials. 

- Invoke gradle build steps 
    - sue gradle wrapper
    - Task: 'build'
    - post buiild archiving with ``` dist/trainSchedile.zipdist/trainSchedile.zip```

Once the project is run we can see that the train schedule zip is there, and this is how a Jenkins pipeline can be implemented, and the build steps can  be sued to buld any type of  code and run any command. 

## Part 2 will be implementing a jenkins Pipeline with a dockerised APP




