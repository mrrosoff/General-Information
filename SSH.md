# SSH - A Walkthrough

## Setting Up SSH On Your Computer

To begin, we want to create the .ssh directory (folder) if it doesn’t already exist. To do this, run the following command. 
mkdir stands for make directory, pretty self-explanatory.

`mkdir .ssh`

If the command fails, it may be because the directory already exists! That is ok, and it's what we want anyways. Next we want to add several 
files into our directory. To enter the directory, use the following command. cd stands for change directory, again pretty self-explanatory.

`cd .ssh`

Next create a config file. Touch creates a file, I have no idea why they named it that.

`touch config`

And then, run the following command to generate a private - public key pair for your computer.

`ssh-keygen -o -a 100 -t ed25519 -f id_example -C "user@example.com"`

This command uses elliptic curve cryptology to secure your key pair. This is much, much more secure than the traditional RSA method. 
You can read more about that [here](https://www.netburner.com/learn/comparing-rsa-and-ecc-encryption/). Now that we have our 
public - private key pair, we can fill up our config file. Open the config file in your terminal editor of choice, mine is nano.

`nano config`

Into this file paste the following. In addition, replace user with your username for the machine you are sshing into. You can read more about 
SSH config files [here](https://linuxize.com/post/using-the-ssh-config-file/). When SSHing into the machine, the config asks to use the IdentityFile 
that we created, our private key. In addition, no matter where we are trying to SSH (*) we are using the Username that you provide, and print 
out any error messages so we know if something went wrong.

```
Host example

	HostName example.com
	IdentityFile ~/.ssh/id_example

Host *

	User user
	IdentitiesOnly yes
	LogLevel VERBOSE
```

If you did this correctly, your setup on your computer is complete! 


## Setting Up SSH On The Server

First, copy your public key from the file by printing it out and storing it in a new tab or file on your desktop. 
cat stands for concatenate, which in this case allows us to simply print out what is in the file.

`cat id_csdept.pub`

Next, ssh into the server. Replace user with your username!

`ssh user@example.com`

Then, navigate to the .ssh folder on your account using cd. If the directory doesn't exist, you may need to make one using the mkdir command from above.
Then create the following file.

`touch authorized_keys`

And paste your key into that file. This can be done a number of different ways, on my machine, I open the file with nano and paste with Ctrl + V

`nano authorized_keys`

Now that you have done this, you are ready to go! Type exit to return to your computer!

`exit`

Testing and Further Configuration

Test the connection by attempting to ssh into albany or atlanta!

`ssh example`

If you are able to login password free, you have been successful!
You can also try transferring a file!

`scp ~/.ssh/id_example albany:~/.ssh/`
