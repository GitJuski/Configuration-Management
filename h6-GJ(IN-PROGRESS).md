# Introduction

This is the report for homework assignment six. As per usual, this assignment is divided into subtasks.

- Task x. Read and summarize. Salt Windows package manager (https://docs.saltproject.io/en/latest/topics/windows/windows-package-manager.html). Parts: Introduction, Install libraries, Populate the local Git repository, Update minion database, Install software package, Usage.

- Task a. Install software to Windows or Mac with SaltStack's pkg.installed function.

- Task b. Search for three to seven configuration management projects from earlier courses. The module should be modern centralized management which means that it's idempotent, it utilizes IaC and the term "singular truth applies". This task doesn't require practical testing. This is more focused on evaluating others' work.

    - Business purpose. How can this be beneficial?

        - Like this (Business purpose): "A robot delivers soda from the fridge to me."
        - Not like this: "ESP32 and Arduino control servos, which dodge objects with LIDAR using a C++ program; the gripping component utilizes a PDI controller..."

    - License.

        - The name of the license.
        - Where can the license be seen.
        - What does the license mean and what is the juridical effect.

    - Creator and the year made.

    - Dependencies: Platform (Operating system, cloud...), network environment etc.

    - Interesting.

        - Useful results
        - interesting technique

    - Open questions and/or other notes.

- Task c. Run state made by someone else.

    - Choose a module from task b. Justify your choice.

    - Check the code.

        - Does it download binaries? Does it download from outside the package manager? Are the sources reputable?

    - Run the code.

    - Test the end result.

    - Take screenshots moderately.

    - Comment and evaluate the module.

    - No need to fix all the found bugs. The purpose is to run the module and comment on it.

    - If a module doesn't seem to work, report your tests and try a different module.

- Task d. List five ideas for your own module. The module should have a purpose, but it doesn't have to solve any real business problem. One sentence per idea.

(Karvinen 2024)

# Task x. Salt's Windows package manager summarized.

- Salt has its own Windows package manager solution. It has a software repository and a package manager. It imitates package manager such as yum and apt.

- The repository is full of package definition files which are normal .sls YAML/JINJA2 files that have all the directives for a software install. The files contain the following:

    - Name of the software
    - Version of the software
    - Download location
    - Command-line switches for silent install and uninstall
    - Whether or not to use the Windows task scheduler to install the package

- The repository does not come with salt by default. That's why the GitHub repository must be cloned first. The repository ([salt-winrepo-ng](https://github.com/saltstack/salt-winrepo-ng)) contains a wide array of these package definition files maintained by SaltStack and the salt community.

- `pkg.install` can be used to install a package. `pkg.installed` checks if the package is already installed first.

- Dependencies must be managed manually while using Salt's Windows package manager.

- Install GitPython or pygit2. This step is optional.

## To populate the local Git repo:

    salt-run winrepo.update_git_repos

or in masterless mode

    salt-call --local winrepo.update_git_repos

## To update the minion database:

    salt -G 'os:windows' pkg.refresh_db

or in masterless mode

    salt-call --local pkg.refresh_db

## To install a software package:

    salt * pkg.install 'firefox_x64'

or in masterless mode

    salt-call --local pkg.install "firefox_x64"

## List installed packages:

    salt -G 'os:windows' pkg.list_pkgs

or in masterless mode

    salt-call --local pkg.list_pkgs

- The software name indicates whether it is managed by salt. The name is usually single-word and lower-case name if it is managed by salt.

## List available versions

    salt winminion pkg.list_available firefox_x64

or in masterless mode

    salt-call --local pkg.list_available firefox_x64

## Install a different version than the latest

    salt winminion pkg.install 'firefox_x64' version=74.0

or in masterless mode

    salt-call --local pkg.install "firefox_x64" version=74.0

- The full name can also be specified

      salt winminion pkg.install 'Mozilla Firefox 17.0.1 (x86 en-US)'

    or 

      salt-call --local pkg.install "Mozilla Firefox 17.0.1 (x86 en-US)"

## Remove a package

    salt winminion pkg.remove firefox_x64

or in masterless mode

    salt-call --local pkg.remove firefox_x64


The whole task x's reference (Saltproject 2024)


# Task a. pkg in Windows.

# Task b. Benchmark.

## Vagrantgoat by Aatu Horelli https://github.com/aatuhorelli/vagrantgoat/

- Business purpose -> Configures a practice environment for testing vulnerabilities.

- License.

    - License name -> GNU General Public License v3.0
    - Where located -> https://github.com/aatuhorelli/vagrantgoat/blob/main/LICENSE
    - Juridical effects:

        - Permissions -> Commercial use, Modification, Distribution, Patent use and Private use

        - Limitations -> Liability and Warranty

- Creator and year created -> Aatu Horelli, 2023

- Dependencies/Requirements:

    - Host machine -> A Linux machine.
    - Tools to have on Host -> Vagrant (VirtualBox), wget and salt-master
    - Environment -> Local
    - Need to manually adjust IP addresses to make it work

- Interesting -> The result is an interesting and useful environment for practice. I liked the usage of scripts with salt modules.

- Open questions and other notes:

    - A good point, as suggested by Aatu is that to make testing more secure, one would want to disconnect the host machine from the internet as well.

## LAMP by Sari Linsen https://github.com/SariLinsen/LAMP/

- Business purpose -> Installs Apache, MariaDB and Python and changes the default page of Apache2. This creates an environment for practice and development purposes at home. In production use, the webserver and database shouldn't be hosted in the same machine.

- License:

    - License name -> GNU General Public License v3.0
    - Where located -> https://github.com/SariLinsen/LAMP/blob/main/LICENSE
    - Juridical effects:

        - Permissions -> Commercial use, Modification, Distribution, Patent use and Private use

        - Limitations -> Liability and Warranty

- Creator and year created -> Sari Linsen, 2022

- Dependencies/Requirements:

    - Platform -> The packages/tools will be installed on the machine in use. A virtual machine is recommended. If you want to utilize script.sh, then you should use a Linux machine and preferably a VM. To use this on a Windows machine, you need to manually do the installations etc.
    - Tools to have before -> (Virtualization tools) and On Windows you should have git and salt-minion.
    - Environment -> local, a vm in the cloud (IaaS).

- Interesting -> The end result is good for testing and practice.

- Other notes -> The module doesn't utilize master-slave architecture. This applies the module's changes to the machine you are currently working on.

## OfficeSetup by Jaakko Sailaranta https://github.com/jakesailaranta/OfficeSetup/

- Business purpose -> Configures a master-slave architecture between different types of minions with essential and useful tools. Development, Media and Webserver.

- License:

    - License name -> GNU General Public License v3.0
    - Where located -> https://github.com/jakesailaranta/OfficeSetup/blob/main/LICENSE
    - juridical effects:

        - Permissions -> Commercial use, Modification, Distribution, Patent use and Private use

        - Limitations -> Liability and Warranty

- Creator and year created -> Jaakko Sailaranta, 2021

- Dependencies/Requirements:

    - Platform -> Master OS must be a Linux distro. Minion OS can be either Windows or Linux.
    - Tools to have before: 
        - Salt-minion must be installed and configured on all minion machines.
        - On Windows minion, the following firewall rules must be enabled: File and Printer Sharing (Echo Request - ICMPv4-In). 
        - Git
    - Environment -> Local, Should work with machines in the cloud as well.

- Interesting -> Brilliant use of variables in init.sls files. Interesting end results with plenty of automation.

- Other notes -> The module is big with essential configurations from software installation to a more fine-tuned approach with wallpaper changes and quality of life enhancements.

# Task c. Testbench.

# Task d. Five ideas.

Innovation isn't really one of my strong suits. Most projects I start are way out of my competence. I'm an ambitious person after all. So, I tried to come up with different ideas that were somewhere along the lines of manageable. The difficulty of these ideas varies, and simple projects can become complicated due to my tendency to dig deep into the rabbit hole.

1. I'm interested in setting up a three-tier architecture. Maybe a module that creates and configures three virtual machines with vagrant (web, app, db) and configures them to work together and within this three-tier architecture.

2. A module that configures an isolated space for security practice.

3. A module that configures a machine that has a website running with Apache & Django. This would be the easier version of the module number 1.

4. A module that configures multiple virtual machines as minions with different purposes. Web, Dev, Test, Docker etc.

5. Maybe a centralized log management solution.

# References

Horelli, A. 2023. Vagrantgoat. Available at https://github.com/aatuhorelli/vagrantgoat/. Read on May 2, 2024.

Karvinen, T. 2024. Infra as Code - Palvelinten hallinta 2024. Available at https://terokarvinen.com/2024/configuration-management-2024-spring/.

Linsen, S. 2022. LAMP. Available at https://github.com/SariLinsen/LAMP/. Read on May 2, 2024.

Sailaranta, J. 2021. OfficeSetup. Available at https://github.com/jakesailaranta/OfficeSetup/. Read on May 2, 2024.

Saltproject. 2024. Windows Package Manager. Available at https://docs.saltproject.io/en/latest/topics/windows/windows-package-manager.html#usage. Read on May 2, 2024.