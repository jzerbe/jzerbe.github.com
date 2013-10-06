---
layout: post
title: CentOS 5.5 ImageMagick install + php module
tags:
- CentOS
- ImageMagick
- php5
published: true
---
Working this morning, I had to get ImageMagick working on a CentOS 5.5 VM. Pretty easy with YUM right?
Well there were some complications installing the associated PHP module. So after doing the usual
`yum update`, here is what you should do.

1. yum install ImageMagick ImageMagick-devel
2. If you are lucky `pecl install imagick` will work, if so skip to last 2 steps.
If not continue along. I got the `$PHP_AUTOCONF` error.
3. Head over to - <http://pecl.php.net/package/imagick> - and download the latest tarball. At time of writing, I used
<http://pecl.php.net/get/imagick-3.0.0RC2.tgz>.
4. tar xzvf [da archive name] and cd into said archive
5. gotta compile this thing from source (check out <http://www.webhostingtalk.com/showpost.php?p=4215404&amp;postcount=10>:
    - `phpize`
    - `./configure`
    - `make`
    - `make install` (as root)
6. load and restart:
    - `echo "extension=imagick.so" > /etc/php.d/imagick.ini`
    - `/etc/init.d/httpd restart`
7. check to see if it was installed: `php -m | grep imagick`

Now let's check to see if imagemagick "works". Create a .php file on the server and access it with
your web browser. Excerpted from <http://www.astahost.com/info.php/Find-Test-Imagemagick-Php_t18769.html>.

    <html> <head> <title>Test for ImageMagick</title> </head>
    <body> <?
    function alist ($array) { //This function prints a text array as an html list.
    $alist = "<ul>";
    for ($i = 0; $i < sizeof($array); $i++) {
    $alist .= "<li>$array[$i]";
    }
    $alist .= "</ul>";
    return $alist;
    }
    exec("convert -version", $out, $rcode); //Try to get ImageMagick "convert" program version number.
    echo "Version return code is $rcode <br>"; //Print the return code: 0 if OK, nonzero if error.
    echo alist($out); //Print the output of "convert -version"
    //Additional code discussed below goes here.
    ?> </body> </html>
