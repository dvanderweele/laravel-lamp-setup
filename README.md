# Setting Up Laravel on a LAMP Stack

NOTE THAT CURRENTLY THIS PROCESS IS UNTESTED, SO DO YOUR OWN RESEARCH, ESPECIALLY FOR PRODUCTION ENVIRONMENTS.

This guide is meant for assisting me in setting up Laravel, mostly on my development machine and in testing environments. You can do whatever you want with it. Obviously if you are deploying a laravel app to production, you may need a more serious take on the security aspect of the endeavor.

Currently this guide uses the following technologies:

- apache2 
- **if you don't like apache and would rather use 
nginx ~~because you're a hipster~~, one of the articles 
below actually uses that)**
- mysql (I just like mysql more 
than mariadb as a slight personal preference, but I'm sure 
you could use that too) 
- php 7.2 
- composer 
- Laravel 5.8 
- Ubuntu 18.04 LTS

I'll do my best to update this repo accordingly as newer versions of the above technologies are released.

## Bibliography:

- [This Medium Article](https://medium.com/@panjeh/install-laravel-on-ubuntu-18-04-with-apache-mysql-php7-lamp-stack-5512bb93ab3f)
- [This Digital Ocean Community Article about LEMP Stack & Laravel](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-laravel-with-lemp-on-ubuntu-18-04)
- [This Digital Ocean Community Article about LEMP & Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-ubuntu-18-04)
- [The Official Laravel Docs](https://laravel.com/docs/5.8)
- [This Digital Ocean Community Article about Composer & Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-18-04)
- [This article about Hardening Apache on Ubuntu](https://hostadvice.com/how-to/how-to-harden-your-apache-web-server-on-ubuntu-18-04/)
- [This Digital Ocean Community Article about Apache and Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04)
- [This Digital Ocean Community Article about Let's Encrypt on Ubuntu 18.04 with Apache](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-18-04)

**DISCLAIMER: I am pretty well convinced of the legal legitimacy under fair use doctrine of the notion that personal notes such as these may be redistributed without fear of violation of copyrights. However, as is the case with all my notes, I am quite amenable to take-down requests.**

