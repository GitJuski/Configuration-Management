# Introduction

The first homework assignment introduces us to saltstack. The assignment is divided into subtasks. These subtasks are the following.

- Task x. Read and summarize. A brief summarization is sufficient. The posts in question are "Run Salt Command Locally", "Create a Web Page Using Github" and "Raportin kirjoittaminen".
- Task a. Show a command that verifies that you have installed salt-minion for Windows or Mac. If you haven't installed it already, install it and report the steps.
- Task b. Show a command that verifies that you have installed Vagrant and VirtualBox.
- Task c. Create a virtual machine with Vagrant.
- Task d. Install salt-minion on the new Linux virtual machine you created with Vagrant.
- Task e. Show examples of the five most important salt states (pkg, file, service, user, cmd). Analyze the findings.
- Task f. provide examples of idempotence. Run 'salt-call --local' commands and analyze the findings. Explain how the idempotence shows.
- Task g. Gather information on the machine with the salt technique 'grains.items'. Pick three interesting parts and show the results (grains.item osfinger virtual) and analyze them.

(Karvinen 2024)

# Essential information

I was in Helsinki, and I used a wireless connection that was mediocre at best.

Here is some information on the machine I used.

![1](/screenshots/1/1.png)

In addition, I had 264GB of free space on my SSD.

# Summarizations

## Run Salt Command Locally summed up

- Salt commands can be run locall, which is good for testing for example. The same commands work in Linux and Windows. Important state functions are pkg, file, service, user and cmd.
- Salt-minion can be installed with `sudo apt-get install salt-minion`
- To check the version -> `sudo salt-call --version`
- pkg.installed is a state where an application should be installed. For example, `sudo salt-call --local -l info state.single pkg.installed tree`. To remove one should change the "installed" part to "removed".
- The state file.managed -> there should be a file. For example, `sudo salt-call --local -l info state.single file.managed /tmp/hellotero`.
- service.running is a state where a demon is running. For example, `sudo salt-call --local -l info state.single service.running apache2 enable=True`.
- user.present is a state where a user should be present. For example, `sudo salt-call --local -l info state.single user.present terote08`.
- cmd.run is for running commands. For example, `sudo salt-call --local -l info state.single cmd.run 'touch /tmp/foo'`. As a note, "Use file, service or user instead of cmd.run".
- Instructions can be found with `sudo salt-call --local sys.state_doc`.

(Karvinen 2021)

## Create a Web Page Using Github

- Steps to creating a web page in Github. Register to Github, create a repo and add README.md, add a new markdown file (.md), write text and commit.
- Public repository, README file to avoid complications and GNU GPL 3 is recommended.
- Add a file by clicking add file and selecting create new file. 
- Add a file name.md.
- Commit to save

(Karvinen 2023)

## Raportin kirjoittaminen

- A report should be as detailed as possible. Report the commands used and the clicks done. Timestamps should be reported as well. What happened? What tests did you do? Report unexpected findings and errors. Write in past tense.
- The report should be repeatable, which means that the results should be the same in the same environment. The environment should be reported.
- The report should be easy to read. Headers and sub headers are important. Proofread and maybe add a summary.
- Write down the references you used.

(Karvinen 2006)

# The practical tasks

## Salt-minion on windows

I started this task on March 28, 2024 at 6:49 PM. First, I opened Windows PowerShell as an administrator. I did it by right clicking the PowerShell icon on my taskbar and selecting run as administrator.

![1](/screenshots/1/2.png)

Then I ran a couple of commands to show that I had Salt installed.

![1](/screenshots/1/3.png)

First command "salt-call --local grains.item osfinger" showed the OS in use.

The second command "ls 'C:\Program Files\Salt Project\' listed the contents of the directory in question. I used tab to complete the path components. There was a directory called Salt that has components. The LastWriteTime shows that I installed it during the lecture.

The last command "Get-service salt-minion" shows that the salt-minion service was running (Microsoft s.a).

I was done at 6:53 PM.

## Vagrant installed, and a new machine created

After a snack I started this task at 7:20 PM. First, I opened the PowerShell and travelled into the Vagrant directory. There I listed the contents, and the Vagrant file tells us that I've already made a configuration. Then I created the machine with vagrant up.

![1](/screenshots/1/4.png)

After the machine was up and running, I connected to the new machine via SSH using vagrant ssh.

![1](/screenshots/1/5.png)

I was done with this at 7:23 PM.

## Salt-minion installation

I started this at 7:27 PM. 

First, I updated and upgraded with `sudo apt update` and `sudo apt upgrade`. Then I installed ufw with `sudo apt install ufw`. I poked a hole with `sudo ufw allow 22/tcp`. Then I enabled the ufw with `sudo ufw enable`. Then I tried to install the salt-minion, but I forgot that I needed to add the repository first. So, I opened a browser and surffed to https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/debian.html.

![1](/screenshots/1/6.png)

I started adding the keyring (saltproject s.a) and an error occurred because I hadn't installed curl first. I installed curl and rerun the command and the other needed command (saltproject s.a).

![1](/screenshots/1/7.png)

After this I updated the repo and installed the minion.

![1](/screenshots/1/8.png)

I was done at 7:34 PM.

## Salt states

I started doing this task at 7:45 PM.

First, I opened my notes of my brief salt experimentations.

![1](/screenshots/1/9.png)

Then I got to work. First, I installed tree with salt (saltproject s.a). Sudo is needed. --local means that the command is run locally. To my understanding the state.single makes it, so a single 'state' is run and in this case it's pkg.installed. When I played around the command wouldn't work without it. pkg.installed is a state where the package should be installed. tree is the package I wanted to install.

![1](/screenshots/1/10.png)

- ID is the package
- Function is the state function. A state where this package is installed.
- Result is True so it was run
- Comment states that the package was installed
- Started and Duration are self explanatory
- Changes list the changes made. Here a new version was installed.
- Summary tells us that nothing failed, and the one thing succeeded and changed. Total states run informs us that it ran once, and total run time is self-explanatory.

Then I created a new file with salt (saltproject s.a).

![1](/screenshots/1/11.png)

- The warning text essentially tells us that I didn't give additional information so 'replace' was set to False instead of True to avoid unnecessary file reading. My interpretation of this is that, when a new file is added to replace an existing file, the function needs to read the file to avoid mistakes.
- ID shows us the file
- Function is the state function. A state where this file is.
- Result is True since it ran
- Comment tells us that the new file was an empty file
- Changes informs us that a new file called hello.txt was added.
- Other information is again self-explanatory

I used `ls` to check that the new file was indeed added.

Here I remembered that I didn't demo the tree.

![1](/screenshots/1/12.png)

Then I checked the SSH service with salt (saltproject s.a).

![1](/screenshots/1/13.png)

- ID shows us the service in question
- Function again tells us the state function.
- Comment informs us that the service was running
- Summary tells us that the command succeeded but nothing was changed.
- Other parts are once again self explanatory

Then I checked if the user "vagrant" was present with salt (saltproject s.a).

![1](/screenshots/1/13.png)

- ID shows the user in question
- Function is the state (user) function (present)
- Comment informs us that the user is indeed present
- Other parts are once again self-explanatory

Then I demoed the cmd state (saltproject s.a).

![1](/screenshots/1/15.png)

- ID shows the command in question
- Function is the state function
- Result is none since nothing was run
- The comment tells us that the command `echo` would have been executed.
- The summary tells us that it succeeded one changed and unchanged. This is a little counter intuitive since the result is none and the comment states that the command "would" have been executed.
- Other parts are once again self explanatory.

Then I did a little demo on YAML files with salt (saltproject s.a).

![1](/screenshots/1/16.png)

![1](/screenshots/1/17.png)

I used the new YAML file, and nothing was changed since I installed the package "tree" earlier.

![1](/screenshots/1/18.png)

I was done at 7:57 PM.

This is where I stopped for that day.

I exited the machine with exit.

![1](/screenshots/1/19.png)

I checked the VirtualBox application to see that the machine is still running.

![1](/screenshots/1/20.png)

Then I googled how to shut down the machine without terminating it through CLI. `vagrant halt` did the job (Hashicorp s.a).

![1](/screenshots/1/21.png)

# Idempotence

It was on March 29, 2024 at 9:40 AM when I was back in business.

I opened PowerShell and tried to resume the halted machine.

![1](/screenshots/1/22.png)

It didn't work so I googled a bit but decided to try solving it first myself. I tried it with the machine ID. The ID probably isn't sensitive information since I'll be terminating this machine eventually. I masked most of the ID anyways just to be safe.

![1](/screenshots/1/23.png)

![1](/screenshots/1/24.png)

Then I grabbed the machine name which could be found inside the VirtualBox app. Tried it with it as well.

![1](/screenshots/1/25.png)

That didn't so I tried vagrant up which worked (stackoverflow 2020). I was under the impression that the command vagrant up creates a new machine. Well, I was wrong.

![1](/screenshots/1/26.png)

Once the machine was up and running, I connected via SSH.

![1](/screenshots/1/27.png)

To show an example of idempotence, I used salt to define a state where the package 'tree' is installed. Because I installed the package yesterday, the function didn't change anything. The package was already installed so nothing was done. To my understanding, this is idempotence.

![1](/screenshots/1/28.png)

I remembered that the file that I created yesterday was owned by root since I did it with sudo salt-call. Now this is not okay (No sudoing in home directory!). So, to change the ownership with salt, I did this (Karvinen March 27, 2024). It informs us that it succeeded, and the user is vagrant, and group is vagrant. This is the one thing that changed to acquire idempotence.

![1](/screenshots/1/29.png)

When running the same command again, we can see that nothing was changed since the file has the proper permissions now. This is idempotence.

![1](/screenshots/1/30.png)

Then I wanted to add text inside the file. This didn't do anything.

![1](/screenshots/1/31.png)

I googled a bit and after a while I decided to try this with an sls file since most answers use these instead of direct commands in the cli. So, I made my way to /srv/salt/ and created a new file there.

![1](/screenshots/1/32.png)

First, I tried to use it with the suffix which didn't work and then without it. I ran into a new problem.

![1](/screenshots/1/33.png)

I checked both of the files side by side since the tree.sls works.

![1](/screenshots/1/34.png)

I found out that the new file didn't have the thing before the function. I think this is called the ID. I added it and ran into a new problem. Well, this tells us that the other problem was fixed!

![1](/screenshots/1/35.png)

I forgot the colon after the ID. I added it and then it worked!

![1](/screenshots/1/36.png)

![1](/screenshots/1/37.png)

It succeeded and changed one thing. I checked the file, and it indeed had the appended text inside.

![1](/screenshots/1/38.png)

To demo idempotence once again, I used the file again and the result informs us that the file is in correct state, and nothing was changed. This is idempotence.

![1](/screenshots/1/39.png)

I was done at 10:23 AM

# Machine information

I started this task at 10:59 AM.

First, I checked the osfinger and virtual results (Karvinen 2024).

![1](/screenshots/1/40.png)

Osfinger informs us on the distro in use which is Debian-12 Bookworm in my case. Virtual informs us on the hypervisor in use.

Then I checked all the results on grains.items. I picked three interesting parts.

![1](/screenshots/1/41.png)

I wanted the three things to only show, so I used grains.item and the three parts cpu_model, num_cpus and saltversion.

![1](/screenshots/1/42.png)

The cpu_model tells us the cpu model in use. The num_cpus tells us how many CPUs are in use. The saltversion tells us the salt version. Other interesting information include different paths etcetera.

I was done at 11:04 AM.

Once I was done with everything, I exited the machine with exit and destroyed the machine with vagrant destroy.

# References

Hashicorp s.a. Halt. Available at https://developer.hashicorp.com/vagrant/docs/cli/halt. Read on March 28, 2024.

Hashicorp s.a. Resume. Available at https://developer.hashicorp.com/vagrant/docs/cli/resume. Read on March 29, 2024.

Karvinen, T 2024. Infra as Code - Palvelinten hallinta 2024. Available at https://terokarvinen.com/2024/configuration-management-2024-spring/.

Karvinen, T 2021. Run Salt Command Locally. Available at https://terokarvinen.com/2021/salt-run-command-locally/. Read on March 29, 2024.

Karvinen, T 2023. Create a Web Page Using Github. Available at https://terokarvinen.com/2023/create-a-web-page-using-github/. Read on March 29, 2024.

Karvinen, T 2006. Raportin kirjoittaminen. Available at https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/. Read on March 29, 2024.

Karvinen, T 2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Available at https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt. Read on March 28, 2024.

Karvinen, T 2018. Salt States – I Want My Computers Like This. Available at https://terokarvinen.com/2018/salt-states-i-want-my-computers-like-this/?fromSearch=salt. Read on March 28, 2024.

Karvinen, T. March 27, 2024. Teacher. The first lecture of the course. Haaga-Helia. Online.

Microsoft s.a. Get-Service. Available at https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.4. Read on March 28, 2024.

Saltproject s.a. Salt install guide Debian. Available at https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/debian.html. Read on March 28, 2024.

Saltproject s.a. Quickstart. Available at https://docs.saltproject.io/salt/install-guide/en/latest/topics/quickstart.html. Read on March 28, 2024.

Saltproject s.a. salt.states.pkg. Available at https://docs.saltproject.io/en/latest/ref/states/all/salt.states.pkg.html. Read on March 28, 2024.

Saltproject s.a. salt.states.file. Available at https://docs.saltproject.io/en/latest/ref/states/all/salt.states.file.html. Read on March 28, 2024.

Saltproject s.a. salt.states.service. Available at https://docs.saltproject.io/en/latest/ref/states/all/salt.states.service.html. Read on March 28, 2024.

Saltproject s.a. salt.states.user. Available at https://docs.saltproject.io/en/latest/ref/states/all/salt.states.user.html. Read on March 28, 2024.

Saltproject s.a. salt.states.cmd. Available at https://docs.saltproject.io/en/latest/ref/states/all/salt.states.cmd.html. Read on March 28, 2024.

Stackoverflow answer 2020. Question header "Should I use vagrant resume or vagrant up?". Answer header "In short". Available at https://stackoverflow.com/questions/25966283/should-i-use-vagrant-resume-or-vagrant-up. Read on March 29, 2024.