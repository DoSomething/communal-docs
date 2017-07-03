# Homestead Setup


_Please refer to the official [Homestead documentation](https://laravel.com/docs/master/homestead) for further information._


## Installation
Before you can get started using Homestead to work on our various DoSomething projects built using Laravel, you need to install a few items.


### Step 1: Install VirtualBox
Download and install the latest version of [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

### Step 2: Install Vagrant
Download and install the latest version of [Vagrant](https://www.vagrantup.com/downloads.html).

### Step 3: Install Homestead
To install Homestead, first clone the [respository](https://github.com/laravel/homestead) to your main code directory, the _home_ directory, or a directory location of your choice. 

These instuctions assume a local `Code` directory within a _home_ directory (i.e. `~/Code`), that houses all code repositories for the various DoSomething projects, but feel free to locate where your heart desires and adjust the command paths accordingly.

```shell
$ cd ~/Code

$ git clone git@github.com:laravel/homestead.git homestead
```

It is recommended to check out the [latest tagged version](https://github.com/laravel/homestead/releases) of Homestead since the `master` branch may not always be stable.

```shell
$ cd ~/Code/homestead

$ git checkout v5.4.0
```

### Step 4: Initialize Homestead
Next, run the `bash init.sh` command from within the Homestead directory, which will create the `Homestead.yaml` configuration file.



## Setup
Once Homestead is initialized, and a `Homestead.yaml` file has been created in the main Homestead repository, you need to configure the file.


### Step 1: Configure Homestead.yaml
Use the following example `Homestead.yaml` to configure your YAML file:

<details>
<summary><strong>Example Homestead.yaml</strong></summary>

```yaml
  ---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox
mongodb: true

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/Code
      to: /home/vagrant/Code

sites:
    - map: gladiator.dev
      to: /home/vagrant/Code/gladiator/public

    - map: longshot.dev
      to: /home/vagrant/Code/longshot/public

    - map: northstar.app
      to: /home/vagrant/Code/northstar/public

    - map: phoenix.dev
      to: /home/vagrant/Code/phoenix/public
    
    - map: rogue.dev
      to: /home/vagrant/Code/rogue/public

databases:
    - gladiator
    - longshot
    - northstar
    - phoenix
    - rogue

# blackfire:
#     - id: foo
#       token: bar
#       client-id: foo
#       client-token: bar

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp

```

</details>

The `folder` key in the `Homestead.yaml` file will `map` the local directory specified `to` the specified path on the virtual machine.

Use the `sites` key to map your DoSomething project "domain" to a folder on your Homestead environment. Make sure to point to the `/public` folder for each project you add.

### Step 2: Configure /etc/hosts
For each of the sites specified in the `Homestead.yaml` file, you need to add those "domains" to the `/etc/hosts` file on you local machine. The `hosts` file will redirect requests for the specified project "domains" into your Homestead machine.

<details>
<summary><strong>Example /etc/hosts</strong></summary>

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1 localhost
255.255.255.255 broadcasthost
::1             localhost 



# DoSomething Projects

192.168.10.10 gladiator.dev
192.168.10.10 longshot.dev
192.168.10.10 northstar.dev
192.168.10.10 phoenix.dev
192.168.10.10 rogue.dev

```

</details>

Make sure that the IP specified (e.g. `192.168.10.10 phoenix.dev`) matches the `ip` key in the `Homestead.yaml` file!

After the Homestead environment is provisioned, each site will be available from the domain specified, such as:

```
http://phoenix.dev
```



## Usage

You can start the virtual machine, head into the directory for the Homestead repository and run:

```shell
$ cd ~/Code/homestead

$ vagrant up
```

Vagrant wil boot the virtual machine and automatically provision the box based on the configurations specified in the `Homestead.yaml` file.

**There's a better way!™**

Instead of having to change directories from a specific project, over to the Homestead repository directory to launch the Vagrant machine (or execute other Vagrant commands), it is recommended to set up an custom bash function that will allow you to run commands on the Homestead vagrant box from anywhere in your filesystem.

Add the following function to either your `.bashrc` or `.bash_profile`:

```bash
function homestead() {
  ( cd ~/Code/homestead && vagrant $* )
}
```

Make sure the path to the homestead repository (`~/Code/homestead`) matches what the actual path of your Homestead installation is on your local machine.

To register the function, either open a new terminal window or run `source ~/.bashrc` or `source ~/.bash_profile`.

This function will now allow you to run any of the vagrant commands as `homestead up`, `homestead ssh` or `homestead halt` etc., from anywhere on your system.
