devbox
======

devbox is a Vagrant development machine provisioned and preconfigured for working with PHP and the [Laravel](http://www.laravel.com) framework out of the box. From nginx, php5.4 over beanstalkd to composer it has got everything you need for Laravel 4.


## Features / Stack
Ubuntu 12.04 32bit, Nginx, PHP5.5, php-fpm, xdebug, composer, MySQL 5.5, PostgreSQL 9.3, Redis, Beanstalkd, supervisord, Sphinx, ngrok, Node.js, MongoDB



## Requirements

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - Free virtualization software 
* [Vagrant](https://www.vagrantup.com) - Tool for working with VirtualBox images


## Initial Setup

* Install VirtualBox and Vagrant ( >= 1.3.0)
* Clone this repository `git clone https://github.com/Aboalarm/devbox.git`. 
* Run `vagrant up` inside the newly created directory. (the first time you run Vagrant it will fetch the virtual box image which is ~300mb. So this could take some time)
* Vagrant will now use Puppet to provision the devbox (this could take a few minutes)
* Point "devbox" and any other vhosts to `192.168.3.3` in your hosts file of your host OS. e.g. `192.168.3.3 devbox myproject.dev myotherproject.dev [HOSTNAME]` 
* Now just clone/copy your Laravel projects into `www/[HOSTNAME]` and open http://[HOSTNAME] in your browser. **Done!** 

## Shared Folders
The www folder is automatically synced to the VM (/var/www). This is why we clone our Laravel project into this folder. The sync works in both directions. So any files generated by Laravel (/storage folder for example) will be accessible from your host machine. 

## Credentials 
* SSH User: `vagrant` PW: `vagrant`
* MySQL User: `root` PW: `root` (access MySQL through SSH)

## Vagrant Commands

* `vagrant up` starts the virtual machine and provisions it
* `vagrant ssh` gives you shell access to the virtual machine
* `vagrant suspend` will essentially put the machine to 'sleep' with `vagrant resume` waking it back up
* `vagrant reload` will reload the VM. Do this when the VM config changed. For exmpale when you changed one of the configs (e.g. php.ini, sphinx.conf, etc. or after a git pull of this repo)
* `vagrant halt` attempts a graceful shutdown of the machine and will need to be brought back with `vagrant up`
* `vagrant halt --force` force shutdown if normal halt doesn't work
* `vagrant destroy` you broke something? this will destroy the VM and reprovisions it again completely. Takes some time.



For more: Vagrant is [very well documented](http://docs.vagrantup.com/v2/)

Please fork, improve, extend, make pull request, wrap it as a gift. Use the GitHub Issues!


## Ngrok 

Ngrok creates a tunnel from the public internet (http://subdomain.ngrok.com) to a website on your local machine. You can give this URL to anyone to allow them to try out a website you're developing without doing any deployment.
For all the features and documentation, check their site: `http://ngrok.com` and usage guide: `http://ngrok.com/usage`.

### Setup:
* In `/etc/nginx/sites-available/ngrok.dev` change `root` path (ie. replace `yoursite.dev` with your site directory)
* Make ngrok configuration active by symlinking it: `sudo ln -s /etc/nginx/sites-available/ngrok.dev /etc/nginx/sites-enabled/ngrok.dev`
* Restart nginx by doing `sudo /etc/init.d/nginx restart`
* Start ngrok service with: `ngrok :80`

## Postgresql

Postgresql service is not running automatically on boot by default. You can run it manually by doing `sudo /etc/init.d/postgresql start`.
You can disable mysql service if it's not in use, to save up some server resources.

## Troubleshoot

* If you use Windows as host OS, disable NFS since it's not supported: edit `Vagrantfile` and set `nfs => false`. On OSX NFS gives much better shared folders performance.
