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
<a href="http://www.debian.org/doc/manuals/apt-howto/ch-basico.en.html">enable non-free and contrib packages</a>.<br />
<br />
Install everything you will need: <code>apt-get install libapache2-mod-chroot libapache2-mod-evasive
    libapache2-mod-fastcgi libapache2-mod-php5 libapache2-mod-python libapache2-mod-suphp libapache2-svn
    libapache2-webauth php-pear unzip php5-cli git-core subversion mercurial mysql-server phpmyadmin ssh</code><br />
<br />
Configure Exim4 properly: <code>dpkg-reconfigure exim4-config</code><br />
<br />
Change sshd to disallow root logins: <code>vi /etc/ssh/sshd_config</code> and change to "PermitRootLogin no".
(On daemon restart, the new settings will take affect.)<br />
<br />
Most of the following pasties are from the <a href="http://projects.ceondo.com/p/indefero/page/Installation/">InDefero docs about installation</a>.<br />
<br />
Install InDefero and Pluf framework files
<blockquote><code>mkdir -p /home/www<br />
        cd /home/www<br />
        wget http://projects.ceondo.com/p/indefero/downloads/32/get/<br />
        unzip index.html<br />
        rm index.html<br />
        mv indefero-1.0 indefero<br />
        wget http://projects.ceondo.com/p/pluf/source/download/master/<br />
        unzip index.html<br />
        rm index.html<br />
        mv pluf-master pluf<br />
        cd /var/www<br />
        rm index.html<br />
        ln -s /home/www/indefero/www/index.php<br />
        ln -s /home/www/indefero/www/media</code></blockquote>
Install/Update PHP Pear shiz
<blockquote><code>pear upgrade-all<br />
        pear install --alldeps Mail<br />
        pear install --alldeps Mail_mime</code></blockquote>
Update Config files (probably don't have to update path.php if similar file structure)
<blockquote><code>cd /home/www/indefero/src/IDF/conf/<br />
        cp idf.php-dist idf.php<br />
        vi idf.php<br />
        cp path.php-dist path.php<br />
        vi path.php</code></blockquote>
Install the Database stuff (first php command tests to see if it will work, second actually installs)
<blockquote><code>cd /home/www/indefero/src<br />
        php /home/www/pluf/src/migrate.php --conf=IDF/conf/idf.php -a -i -d -u<br />
        php /home/www/pluf/src/migrate.php --conf=IDF/conf/idf.php -a -i -d</code></blockquote>
Bootstrap this bumkin!<br />
vi bootstrap.php and stick the following in (if your path, etc. are the same):
<blockquote><div id="_mcePaste">&lt;?php</div>
    <div id="_mcePaste">require '/home/www/indefero/src/IDF/conf/path.php';</div>
    <div id="_mcePaste">require 'Pluf.php';</div>
    <div id="_mcePaste">Pluf::start('/home/www/indefero/src/IDF/conf/idf.php');</div>
    <div id="_mcePaste">Pluf_Dispatcher::loadControllers(Pluf::f('idf_views'));</div>
    <div id="_mcePaste">$user = new Pluf_User();</div>
    <div id="_mcePaste">$user-&gt;first_name = 'John';</div>
    <div id="_mcePaste">$user-&gt;last_name = 'Doe'; // Required!</div>
    <div id="_mcePaste">$user-&gt;login = 'doe'; // must be lowercase!</div>
    <div id="_mcePaste">$user-&gt;email = 'doe@example.com';</div>
    <div id="_mcePaste">$user-&gt;password = 'yourpassword'; // the password is salted/hashed</div>
    <div id="_mcePaste">// in the database, so do not worry :)</div>
    <div id="_mcePaste">$user-&gt;administrator = true;</div>
    <div id="_mcePaste">$user-&gt;active = true;</div>
    <div id="_mcePaste">$user-&gt;create();</div>
    <div id="_mcePaste">print "Bootstrap okn";</div>
    <div id="_mcePaste">?&gt;</div></blockquote>
Run that sucker - <code>php bootstrap.php</code> - make sure it completes, and then delete it (<code>rm bootstrap.php</code>).<br />
<br />
Finishing Touches<br />
cd /var/www<br />
vi .htaccess - and put the following in if you want urls without index.php in them:
<blockquote><code>Options +FollowSymLinks<br />
        RewriteEngine On<br />
        RewriteCond %{REQUEST_FILENAME} !-f<br />
        RewriteCond %{REQUEST_FILENAME} !-d<br />
        RewriteRule ^(.*) /index.php/$1</code></blockquote>
Don't forget to enable to rewrite module - <code>a2enmod rewrite</code> - and restart apache2 - <code>/etc/init.d/apache2 restart</code>.<br />
<br />
Adding automatic control over your repos:<br />
Subversion - <a href="http://projects.ceondo.com/p/indefero/page/InstallationScmSubversion/">http://projects.ceondo.com/p/indefero/page/InstallationScmSubversion/</a><br />
Git - <a href="http://projects.ceondo.com/p/indefero/page/InstallationScmGit/">http://projects.ceondo.com/p/indefero/page/InstallationScmGit/</a><br />
Mercurial - <a href="http://projects.ceondo.com/p/indefero/page/InstallationScmMercurial/">http://projects.ceondo.com/p/indefero/page/InstallationScmMercurial/</a><br />
Automatic start of the Git daemon - <a href="http://projects.ceondo.com/p/indefero/page/git-daemon-sysV-InitScript/">http://projects.ceondo.com/p/indefero/page/git-daemon-sysV-InitScript/</a><br />
