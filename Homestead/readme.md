# Homestead Setup

> Laravel Homestead is an official, pre-packaged Vagrant box that provides you a wonderful development environment without requiring you to install PHP, a web server, and any other server software on your local machine.

We use it to ensure everyone on the team has a consistent environment & can get up and running quickly!

_Here's a "quick start" guide to using Homestead. Refer to the [official documentation](https://laravel.com/docs/master/homestead) for more information._ :books:

## Installation

#### Step 1: Install VirtualBox & Vagrant
Download and install the latest version of [VirtualBox 6.x](https://www.virtualbox.org/wiki/Downloads) & [Vagrant](https://www.vagrantup.com/downloads.html).

#### Step 2: Install Homestead
To install Homestead, first clone the [repository](https://github.com/laravel/homestead) to wherever you store your source code (or any other location of your choice). These instructions assume you're storing your projects in `~/Code`.

```shell
$ cd ~/Code

$ git clone git@github.com:laravel/homestead.git homestead
```

Check out the [`v12.1.0` tag](https://github.com/laravel/homestead/releases) of Homestead. We'll periodically test newer releases and update these directions.

```shell
$ cd ~/Code/homestead

$ git checkout v12.1.0
```

#### Step 3: Configure Homestead
Next, run the `bash init.sh` command from within the Homestead directory, which will create the `Homestead.yaml` configuration file & `after.sh` shell script.

You can use the `Homestead.yaml` file to configure which sites run in your Homestead environment. Here's an example, configured for working on [Northstar](https://github.com/dosomething/northstar), [Phoenix](https://github.com/dosomething/phoenix-next), [Rogue](https://github.com/dosomething/rogue), [Aurora](https://github.com/dosomething/aurora), and [Chompy](https://github.com/dosomething/chompy):

```yaml
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

# Install MongoDB & MariaDB.
features:
    - mongodb: true
    - mariadb: true

# Configure which folders on your local machine
# are accessible on the Homestead VM. This should
# include an entry for whichever folders you store
# your source code in.
#
#  - map: <PATH ON YOUR MAC/PC>
#    to: <PATH ON YOUR VAGRANT BOX>
#
folders:
    - map: ~/Code
      to: /home/vagrant/Code

# Configure which Laravel applications you are running,
# and where their "public" directory can be found. Make
# sure the 'to' path begins with one of the folders
# you linked above.
sites:
    - map: aurora.test
      to: /home/vagrant/Code/aurora/public
      php: "7.4"

    - map: northstar.test
      to: /home/vagrant/Code/northstar/public
      php: "7.4"

    - map: phoenix.test
      to: /home/vagrant/Code/phoenix-next/public
      php: "7.4"
    
    - map: rogue.test
      to: /home/vagrant/Code/rogue/public
      php: "7.4"
      
    - map: chompy.test
      to: /home/vagrant/Code/chompy/public
      php: "7.4"

# These databases will automatically be created by
# Homestead when provisioning your virtual machine.
databases:
    - aurora
    - chompy
    - chompy_test
    - phoenix
    - phoenix_test
    - rogue
    - rogue_test

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp
```

Finally, make one change to the `after.sh` file:

```shell
#!/bin/sh

# If you would like to do some extra provisioning you may
# add any commands you wish to this file and they will
# be run after the Homestead machine is provisioned.

# Default to PHP 7.4 CLI:
sudo update-alternatives --set php /usr/bin/php7.4
sudo update-alternatives --set php-config /usr/bin/php-config7.4
sudo update-alternatives --set phpize /usr/bin/phpize7.4

# Set 10MB nginx upload limit:
echo 'client_max_body_size 10M;' >> /home/vagrant/.config/nginx/nginx.conf

# Use "polling" rather than unsupported filesystem events for tools like Webpack:
echo 'export CHOKIDAR_USEPOLLING=true' >> /home/vagrant/.bashrc

# Install New Relic agent:
sudo sh -c "echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' > /etc/apt/sources.list.d/newrelic.list"
wget -O- https://download.newrelic.com/548C16BF.gpg | sudo apt-key add -
sudo apt-get update

sudo DEBIAN_FRONTEND=noninteractive apt-get -y install newrelic-php5
```

#### Step 4: Configure /etc/hosts
For each of the sites specified in the `Homestead.yaml` file, you need to add those domains to the `/etc/hosts` file on your local machine. This will route requests for the specified URLs into your Homestead machine.

```shell
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1 localhost
255.255.255.255 broadcasthost
::1             localhost 


# Homestead Projects
192.168.10.10 aurora.test
192.168.10.10 northstar.test
192.168.10.10 phoenix.test
192.168.10.10 rogue.test
192.168.10.10 chompy.test
```

Make sure that the IP specified (e.g. `192.168.10.10 phoenix.test`) matches the `ip` key in the `Homestead.yaml` file!


#### Step 5: Let's do this!

You should be ready to go! 

If this is your first time working on an application, head over to the project directory within your Homestead box (following the "Daily Usage" instructions below) and follow the project's installation instructions in its README file to install dependencies, run database migrations, and build any front-end assets.

Each of your sites should now be accessible in a web browser, like [http://phoenix.test](http://phoenix.test)! Have fun! :sparkles:


## Daily Usage
You can start your virtual machine from the `~/Code/homestead` directory you created above.

```shell
$ cd ~/Code/homestead

# To start your Homestead box:
$ vagrant up

# To SSH into your Vagrant box (for running commands
# like `composer` or `php artisan`):
$ vagrant ssh

# To stop your Homestead box:
$ vagrant stop
```

If you've made any changes to your `Homestead.yaml` file, Vagrant will automatically make the corresponding changes on your virtual machine. You can also do this by running `vagrant provision` from this directory at any time.

#### There's a better way!™
It can be frustrating to have to always switch between your Homestead directory and other project directories. Luckily, you can save some time by making a `homestead` command that lets you interact with your Homestead virtual machine from anywhere:

Add the following function to your `.bashrc` or `.bash_profile`:

```bash
function homestead() {
  DIRECTORY=$(pwd)
  HOMESTEAD_DIRECTORY="$HOME/Code/homestead"
  HOME_RELATIVE_DIRECTORY=${DIRECTORY/$HOME/\~}
  DEFAULT="ssh --command \"cd $HOME_RELATIVE_DIRECTORY; bash\""
  (cd $HOMESTEAD_DIRECTORY; eval "vagrant ${*:-$DEFAULT}")
}
```

Make sure the path to the homestead repository (`~/Code/homestead`) matches the path you chose when installing Homestead installation on your local machine. After saving those changes, either open a new terminal window or run `source ~/.bashrc` or `source ~/.bash_profile`.

You can now run `homestead up`, `homestead ssh` or `homestead halt` from anywhere!

## Contributing

Be sure to commit changes to each repository from outside of your vagrant box, so your Github user will be the author (not `vagrant`).

Follow [these steps](https://help.github.com/en/articles/setting-your-username-in-git#setting-your-git-username-for-every-repository-on-your-computer) to set your Github username for every repository.

## Troubleshooting
If you encounter any issues while installing & using Homestead, refer to the [troubleshooting guide](troubleshooting.md).
