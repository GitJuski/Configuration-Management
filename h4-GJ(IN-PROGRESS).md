# Introduction

This is the report for homework assignment number four. The assignment is divided into subtasks. The tasks are the following:

- Task x. Read and summarize. A couple of bullet points per post. The posts are the following:

    - Salt Vagrant - automatically provision one master and two slaves, parts Infra as Code - Your wishes as a text file and top.sls - What Slave Runs What States by Karvinen 2023.

    - Salt overview, parts Rules of YAML, YAML simple structure and Lists and dictionaries - YAML block structures by Salt contributors.

    - state docs for pkg, file and service. The homework directive states that a glance through is sufficient enough.

- Task a. Hello sls! Create a hello world state by writing it into a text file.

- Task b. Top. Create a top.sls to run multiple states automatically.

- Task c. Apache easy mode. Install Apache, replace the default site and verify that the demon starts.

    - First manually by hand
    - Write the state into an sls-file
    - pkg-file-service
    - No need to use watch with the service since the index.html isn't a config file. 

- Task d. SSH. Add a new port for the sshd to listen to.

- Task e. Optional. Install Apache to host a website. The site should be available to localhost. HTML should be inside some users home directory and a normal user should be able to modify it.

- Task f. Optional. Same as previous but with Caddy instead of Apache.

- Task g. Optional. Same as previous but with Nginx instead of Apache or Caddy.

(Karvinen 2024)

# Written tasks (Task x)

## automatically provision one master and two slaves

COMING SOON...

## Salt overview, parts Rules of YAML, YAML simple structure and Lists and dictionaries

COMING SOON...

## pkg, file and service

COMING SOON...

# Practical tasks

## Essential information

- Latest installed feature update: Windows 11, 23H2

- Location: Vaasa

- Network: Decent wireless connection

- Starting date: April 18, 2024

- Storage: SSD with 255 GB of free space

- Rest of the information down below

![1](screenshots/1/1.png)

## Task a. Hello SLS!

I started this task at 11:21 AM.

I opened PowerShell by clicking the icon on taskbar. I created a new directory inside my vagrant directory. Then I created a new machine config.

![1](screenshots/4/1.png)

Then I used `vagrant up` to provision the machine. This took a while. The connection reset a couple of times. Once it was done, I used `vagrant ssh` to connect to the new machine. There I did some usual steps.

![1](screenshots/4/2.png)

- I usually use the command `clear` instead of `Ctrl + L` since I've found it to be a faster method for me. Since I wanted to show the history, I used `Ctrl + L` this time to show a cleaner history. Unfortunately out of habit, I used `clear` as the first command.

- The second command updated the package repos and upgraded the packages I had.

- The third command installed ufw (uncomplicated firewall)

- The fourth command created a hole for port 22 and enabled the firewall.

- The fifth command installed the following packages: micro, curl and salt-minion.

- The sixth command shows the history.

After this, I created a new directory, made the micro the default text editor in this session and created a new file.

![1](screenshots/4/3.png)

The task says that we are to create a hello word state. I wasn't sure on what kind of state I needed to create. `cmd.run` was an option, but since it is the last resort, I decided to create a `file.state`. This state creates a file helloworld file.

![1](screenshots/4/4.png)

Before this, I tested that the salt-minion worked with `sudo salt-call --local grains.item osfinger`. Then I tested the newly created state.

![1](screenshots/4/5.png)

It worked, but I noticed that the file was created inside the root's home directory. At this point I remembered that the salt commands run as root and since I wrote the path like this `~/helloworld.txt` it created the file inside the root's home directory.

I fixed this by writing the absolute path.

![1](screenshots/4/7.png)

I also changed the micro's colorscheme. This can be done like this.

1. Open micro
2. Ctrl + E
3. write: set colorscheme
4. With tab you can browse the available schemes
5. choose one and hit enter

![1](screenshots/4/6.png)

I also removed the earlier file with `sudo rm /root/helloworld.txt`. I tried the fixed state.

![1](screenshots/4/8.png)

Then I checked the contents.

![1](screenshots/4/9.png)

I was done at 11:38 AM.

## Task b. Top.

I started this task at 12:06 PM

I created a top.sls file.

![1](screenshots/4/10.png)

I wrote this.

![1](screenshots/4/11.png)

- The first line: base is the place where salt looks for the states stated below.
- The second line: '*' the asterix means that these states will be applied to all minions. A singular minion or a group of minions could be stated here. `'webserver1'` -> states will apply to minion named webserver1. `'web*'` means that the states will apply to minions whose name starts with web.
- The last line is the state or modules to be applied.

Test didn't work.

![1](screenshots/4/12.png)

The line 2 had `'*'` instead of `'*':`.

After fixing it, the test worked.

![1](screenshots/4/13.png)

Then I created two new modules. `sudo mkdir tree ssh_state` and `sudoedit tree/init.sls` and `sudoedit ssh_state/init.sls`.

![1](screenshots/4/14.png)

Then I opened the top.sls with `sudoedit top.sls`. I added the new modules.

![1](screenshots/4/15.png)

Test didn't produce wanted output since I apparently used tab instead of spaces.

![1](screenshots/4/16.png)

After fixing the indentation, the test was successful.

![1](screenshots/4/17.png)

I was done at 12:15 PM

## Task c. Apache easy mode.

I started this task at 12:30 PM.

I created a new directory inside the home directory and created a new config file.

![1](screenshots/4/18.png)

Contents of the config file.

![1](screenshots/4/19.png)

Then I created an index.html inside the newly created directory.

![1](screenshots/4/20.png)

Disabled the old conf and enabled the new conf. Restarted Apache2 demon.

![1](screenshots/4/21.png)

Checked the localhost.

![1](screenshots/4/22.png)

I was done with the manual part at 12:35 PM.

Here I realized that I wasn't supposed to do this much. I only needed to change the default page.

I enabled the old conf again.

![1](screenshots/4/23.png)

Replaced the default page with text 'default page missing'. Then I checked the results.

![1](screenshots/4/24.png)

I was done with manual part 2 at 12:37 PM.

After a tiny break, I continued at 12:56 PM.

I started by removing Apache and removing the /var/www/ and /etc/apache2/ directories.

![1](screenshots/4/25.png)

Then I created a new module and a new file.

![1](screenshots/4/26.png)

I started off small.

![1](screenshots/4/27.png)

Tested it and it works.

![1](screenshots/4/28.png)

Then I added the file and service states as well.

![1](screenshots/4/29.png)

I tested it and it worked. The only thing that changed was the file part. It replaced the contents with the 'Default page missing' text.

![1](screenshots/4/30.png)

Then I checked that it was idempotent.

![1](screenshots/4/31.png)

I was done at 1:05 PM.

I wanted to try it without Apache installed so, I used purge once more to remove the demon and deleted the /var/www/ directory.

It worked as intended.

![1](screenshots/4/32.png)

Here's the output: [test_output_h4.txt](miscellaneous/test_output_h4.txt)

## Task d. SSH.

Back in business at 3:26 PM.

Here I demonstrated where to find the file and the part. I've changed a couple of things in the sshd_config before, so this was nothing new to me.

![1](screenshots/4/33.png)

I guess there's a couple of ways to do this. I wanted to make it idempotent so I took an approach where I just copied the current config and used the copy as a source.

![1](screenshots/4/34.png)

After I copied it, I added a new port after port 22. `sudoedit ssh_state/config`

![1](screenshots/4/35.png)

Then I opened the init.sls with `sudoedit ssh_state/init.sls`. I added a new state with the ID configure. Then I modified the other state I had from before.

- The file.managed state here creates an sshd_config file and uses the config file as the source for the contents.

- The service.running state here checks that the sshd service is running and the watch makes it so that the service is restarted when the other state is applied. This means that, when changes are made to the source file the service is restarted.

![1](screenshots/4/36.png)

The Test was successful

![1](screenshots/4/37.png)

Here I checked that the port was added.

![1](screenshots/4/38.png)

Here I checked the sshd is listening on 1234 and 22.

![1](screenshots/4/39.png)

Then I changed removed the port from the source file by editing it with `cd ssh_config/` and `sudoedit config`. Then I tested it again with `sudo salt-call --local state.apply ssh_state`. After the run, I checked that the demon restarted.

![1](screenshots/4/40.png)

I was done at 3:37 PM.

## Task e. Optional Apache.

I've done something like this before this was taught. I wrote a report on it a couple of assignments back. It can be found here https://github.com/GitJuski/Configuration-Management/blob/main/h2-GJ.md#task-f-hello-iac. This was the second homework assignment. I tried to use the new information and adapt. The results of this week's task were much better compared to the one before.

I started this at 3:58 PM.

First, I made my way to my module I created earlier. I created a file that I'll be using as a source.

![1](screenshots/4/41.png)

I wrote three states.

![1](screenshots/4/42.png)

The test was not successful. The problem was that I used config instead of conf as the source.

![1](screenshots/4/43.png)

After fixing it the output was good. I checked that the directory and the file was created.

![1](screenshots/4/44.png)

Then I edited the file some more with `sudoedit init.sls`.

![1](screenshots/4/45.png)

- I changed the user and group to be owned by vagrant on the file.
- I added a state that makes sure that the default conf is disabled. The watch here was unneccessary. I had something in mind that I forgot soon after.

The output was once again good.

![1](screenshots/4/46.png)

Since I created the website directory the first time without any though, I had to change the ownership. I decided to do it by hand. This could be done with salt as well.

![1](screenshots/4/47.png)

Then I added a state where the correct config is enabled.

![1](screenshots/4/48.png)

An error occured.

![1](screenshots/4/49.png)

My memory was at fault here. I used `path:` instead of `target:`. I fixed it and it worked.

Then I added the service state. I also gave some other states a watch_in statement (Karvinen 2024). With this the Apache2 demon is restart every time the config is changed, default config is disabled or the new config is enabled.

![1](screenshots/4/50.png)

Then I tested my IaC again but from a clean slate. I removed Apache with `sudo apt purge apache2`, removed directories and files with `sudo rm -rf /var/www/ /etc/apache2/ ~/website/`. Then I tested it and the results were great.

![1](screenshots/4/51.png)

Here's the whole output: [test2_h4.txt](miscellaneous/test2_h4.txt)

And here's the second test where I tested idempotence (Karvinen 2024).

![1](screenshots/4/52.png)

Here I checked the end results.

![1](screenshots/4/53.png)

I was done at 4:28 PM

## Task f. Optional Caddy.

COMING SOON...

## Task g. Optional.

COMING SOON...

# References

Karvinen, T. 2024. Infra as Code - Palvelinten hallinta 2024. Available at https://terokarvinen.com/2024/configuration-management-2024-spring/.

Karvinen, T. April 17, 2024. Lecture. 