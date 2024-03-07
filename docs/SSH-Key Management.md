# SSH Key Management

## Introduction

SSH key management involves the generation, distribution, and secure storage of SSH (Secure Shell) keys used for authentication between systems.
It contains two parts i.e Public key(id_rsa.pub) and Private key(id_rsa). Public Key from SSH is inserted into the github account under the SSH Key in setting and Private Key from SSH is stored into the host system to authenticate the user.

##  Ways to generate SSH Key Pair on UNIX and UNIX-Like Systems
<ol>
<li> Run the ssh-keygen command. </li>

```
ssh-keygen -t rsa  //Works for simply generating ssh key pair
   
or
   
ssh-keygen -t rsa -f ~/.ssh/<filename> -C <email_address> //It will generate ssh key pair with filename for particular email
   
Example:
ssh-keygen -t rsa -f ~/.ssh/personal -C personal@gmail.com
```
   You can use the -t option to specify the type of key to create. You can also generate ssh key pair using other algorithm rather than only rsa.

<li> The command prompts you to enter the path to the file in which you want to save the key.</li>

   A default path and file name are suggested in parentheses. For example:
```
/home/<user_name>/.ssh/id_rsa 
```
   To accept the default path and file name, press Enter. Otherwise, enter the required path and file name, and then press Enter.

<li> The command prompts you to enter a passphrase. </li>

   The passphrase is not mandatory. However, it is recommended that you specify a passphrase to protect your private key against unauthorized use.
    
<li> When prompted, enter the passphrase again to confirm it.</li>
<li> Test SSH connection: </li>
</ol>
    
    To ensure that everything is setup correctly, test the SSH connection to Github.
```
ssh -T git@github.com
```

The command generates an SSH key pair consisting of a public key and a private key, and saves them in the specified path. The file name of the public key is created automatically by appending .pub to the name of the private key file. For example, if the file name of the SSH private key is id_rsa, the file name of the public key would be id_rsa.pub.

Now, You need to paste the id_rsa.pub key from .ssh folder into the github account under the SSH key section in github setting.

## REFERENCE LINK
<a href="https://docs.oracle.com/en/cloud/cloud-at-customer/occ-get-started/generate-ssh-key-pair.html#GUID-8B9E7FCB-CEA3-4FB3-BF1A-FD3406A2432F">Generate SSH</a>


## Knowledge Based
Problems you may encounter during ssh setup

### Problem 1: Pasting private key in Github instead of Public key from ssh

Remember that in .ssh folder there are two file upon the ssh key pair generation. Private key is with file name id_rsa and public key is with file name id_rsa.pub.

Insert the contents of id_rsa.pub into the github account under the section settings/SSH and GPG keys 

### Problem 2: Generate multiple SSH for using multiple github account in one System

```
ssh-keygen -t rsa -f ~/.ssh/<filename> -C <email_address>

Example:
ssh-keygen -t rsa -f ~/.ssh/personal -C personal@gmail.com
```

Create config file in .ssh folder and write these lines in config file.
```
Host github.com
  Hostname github.com
  IdentityFile ~/.ssh/id_rsa

Host github.com-personal
  Hostname github.com
  IdentityFile ~/.ssh/personal
```
Then Paste this in the cmd

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/personal
```

Move to your project and setup git user config
```
cd <project_folder> // Replace project_folder with your own project_folder
git config user.email "<email_address>" // Replace email_address with your own email_address
git config user.name "<user_name>" // Replace user_name with your own user_name

Example:
cd cloco_project
git config user.email "cloco@gmail.com"
git config user.name "cloco"
```

### Problem 3: Please make sure you have the correct access rights
You might get this error when you have setup multiple ssh key

TO fix this issue first make sure that you have initialized the git by
```
git init
```
Then make sure to add the git email and name in that particular git
```
git config user.email "<email_address>" // Replace email_address with your own email_address
git config user.name "<user_name>" // Replace user_name with your own user_name

Example:
git config user.email "cloco@gmail.com"
git config user.name "cloco"
```

Then paste this code:
```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/personal //Make sure to change it according to the your git private key of that repo
```

Now you can clone or push your repo of which ssh key is added to the github.
