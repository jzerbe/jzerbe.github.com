---
layout: post
title: InDefero on Debian Lenny with subversion, git, mercurial
tags:
- InDefero
- Debian
- subversion
- git
- mercurial
published: true
---
I needed to host some serious source code repositories on one of my own systems (for IP and security reasons),
plus I was also sick of the periodic downtime from using free services. I did the install on a fresh
VMware (Server) Debian Lenny virtual machine on an old Dell Poweredge; did the initial install
from the latest (at time of writing) netinstall image. You should probably
[enable non-free and contrib packages](http://www.debian.org/doc/manuals/apt-howto/ch-basico.en.html).

Install everything you will need: _apt-get install libapache2-mod-chroot libapache2-mod-evasive
    libapache2-mod-fastcgi libapache2-mod-php5 libapache2-mod-python libapache2-mod-suphp libapache2-svn
    libapache2-webauth php-pear unzip php5-cli git-core subversion mercurial mysql-server phpmyadmin ssh_

Configure Exim4 properly: `dpkg-reconfigure exim4-config`

Change sshd to disallow root logins: `vi /etc/ssh/sshd_config` and change to _PermitRootLogin no_.
(On daemon restart, the new settings will take affect.)

Most of the following pasties are from the [InDefero docs about installatio](http://projects.ceondo.com/p/indefero/page/Installation/).


####Install InDefero and Pluf framework files

    mkdir -p /home/www
    cd /home/www
    wget http://projects.ceondo.com/p/indefero/downloads/32/get/
    unzip index.html
    rm index.html
    mv indefero-1.0 indefero
    wget http://projects.ceondo.com/p/pluf/source/download/master/
    unzip index.html
    rm index.html
    mv pluf-master pluf
    cd /var/www
    rm index.html
    ln -s /home/www/indefero/www/index.php
    ln -s /home/www/indefero/www/media


####Install/Update PHP Pear shiz

    pear upgrade-all
    pear install --alldeps Mail
    pear install --alldeps Mail_mime


####Update Config file

    cd /home/www/indefero/src/IDF/conf/
    cp idf.php-dist idf.php
    vi idf.php
    cp path.php-dist path.php
    vi path.php


####Install the Database stuff

    cd /home/www/indefero/src
    php /home/www/pluf/src/migrate.php --conf=IDF/conf/idf.php -a -i -d -u
    php /home/www/pluf/src/migrate.php --conf=IDF/conf/idf.php -a -i -d


####Bootstrap

`vi bootstrap.ph` and stick the following in (if your path, etc. are the same):

`<?php`

    require '/home/www/indefero/src/IDF/conf/path.php';
    require 'Pluf.php';
    Pluf::start('/home/www/indefero/src/IDF/conf/idf.php');
    Pluf_Dispatcher::loadControllers(Pluf::f('idf_views'));
    $user = new Pluf_User();
    $user->first_name = 'John';
    $user->last_name = 'Doe'; // Required!
    $user->login = 'doe'; // must be lowercase!
    $user->email = 'doe@example.com';
    $user->password = 'yourpassword'; // the password is salted/hashed
    // in the database, so do not worry :)
    $user->administrator = true;
    $user->active = true;
    $user->create();
    print "Bootstrap ok";

    ?>

Run that sucker - `php bootstrap.php` - make sure it completes, and then delete it (`rm bootstrap.php`).


####Finishing Touches

`cd /var/www`
`vi .htaccess` - and put the following in if you want urls without index.php in them:

    Options +FollowSymLinks
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*) /index.php/$1

Don\'t forget to enable to rewrite module - `a2enmod rewrite` - and restart apache2 - `/etc/init.d/apache2 restart`.


####Adding automatic control over your repos

- Subversion - <http://projects.ceondo.com/p/indefero/page/InstallationScmSubversion/>
- Git - <http://projects.ceondo.com/p/indefero/page/InstallationScmGit/>
- Mercurial - <http://projects.ceondo.com/p/indefero/page/InstallationScmMercurial/>
- Automatic start of the Git daemon - <http://projects.ceondo.com/p/indefero/page/git-daemon-sysV-InitScript/>
