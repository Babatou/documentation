Here is a tutorial on how to install openjabnab locally (on a rpi or
humming)

> ** Note **
>
> This tutorial is largely inspired by
> [this one] (http://jetweb.free.fr/nabaztag_rpi/Tutoriel_OJN_RPi_v1-1.pdf)

Installing dependencies
============================

Once the system installed in SSH done:

    apt-get update
    apt-get dist-upgrade
    apt-get install ssh
    apt-get install php5 apache2 php5-mysql libapache2-mod-php5
    a2enmod rewrite
    apt-get install make
    apt-get install build-essential
    apt-get install libqt4-dev --fix-missing
    apt-get install qt4-dev-tools
    apt-get install bind9
    apt-get install git

Network configuration
=======================

It is then necessary to recover the IP address of the system:

    ifconfig

The result is:

    eth0 Link encap: Ethernet HWaddr d0: 63: b4: 00: 54: 98
              inet addr: 192.168.0.162 Bcast: 192.168.0.255 Mask: 255.255.255.0
              inet6 addr: fe80 :: d263: b4ff: fe00: 5498/64 Scope: Link
              UP BROADCAST RUNNING MULTICAST MTU: 1500 Metric: 1
              RX packets: 10721 errors: 0 dropped: 0 overruns: 0 frame: 0
              TX packets: 6477 errors: 0 dropped: 0 overruns: 0 carrier: 0
              collisions: 0 txqueuelen: 1000
              RX bytes: 2032942 (1.9 MiB) TX bytes: 1230703 (1.1 MiB)

Here the IP address is 192.168.0.162.

> ** Note **
>
> For the rest of the tutorial I will use this IP, it is of course to
> replace according to the one you actually have

Then edit the file /etc/resolv.conf

    vim /etc/resolv.conf

And add:

    nameserver 192.168.0.162

DNS configuration
====================

Edit the file /etc/bind/named.conf.local

    cd / etc / bind /
    vim named.conf.local

And add:

    zone "raspberry.pi" {
     master type;
     file "/etc/bind/db.raspberry.pi";
    };
    zone "0.168.192.in-addr.arpa" {
     master type;
     file "/etc/bind/db.192.168.0.inv";
    };

Create the file db.raspberry.pi

vim db.raspberry.pi ---

And put in:

    $ TTL 604800
    @ IN SOA ojn.raspberry.pi. root.raspberry.pi. (
     1; Serial
     604800; Refresh
     86400; Retry
     2419200; Expired
     604800); Negative TTL Cache
    ;
    @ IN NS ojn.raspberry.pi.
    ojn IN A 192.168.0.162
    192.168.0.162 IN A 192.168.0.162

Then create this file db.192.168.0.inv

    vim db.192.168.0.inv

And put:

    $ TTL 604800
    @ IN SOA ojn.raspberry.pi. root.localhost. (
     2; Serial
     604800; Refresh
     86400; Retry
     2419200; Expired
     604800); Negative TTL Cache
    ;
    @ IN NS ojn.raspberry.pi.
    162 IN PTR ojn.raspberry.pi.

> ** Important **
>
> Remember to replace the 162 in the last line with the last
> part of the ip of your system

Launch the DNS:

    /etc/init.d/bind9 start

Test if it's good:

    ping ojn.raspberry.pi

You should have :

    root @ cubox-i: / home / ojn # ping ojn.raspberry.pi
    PING ojn.raspberry.pi (192.168.0.162) 56 (84) bytes of data.
    64 bytes from ojn.raspberry.pi (192.168.0.162): icmp_seq = 1 ttl = 64 time = 0.069 ms
    64 bytes from ojn.raspberry.pi (192.168.0.162): icmp_seq = 2 ttl = 64 time = 0.067 ms
    64 bytes from ojn.raspberry.pi (192.168.0.162): icmp_seq = 3 ttl = 64 time = 0.059 ms
    64 bytes from ojn.raspberry.pi (192.168.0.162): icmp_seq = 4 ttl = 64 time = 0.068 ms
    ^ C
    --- ojn.raspberry.pi ping statistics ---
    4 packets transmitted, 4 received, 0% packet loss, time 3000ms
    rtt min / avg / max / mdev = 0.059 / 0.065 / 0.069 / 0.010 ms

> ** Note **
>
> You have to do ctrl + c to exit the ping

For security we will also add the resolution in / etc / hosts, do:

    vim / etc / hosts

And add:

    192.168.0.162 ojn.raspberry.pi

Openjabnab recovery
=========================

We will first create the user:

    adduser ojn
    cd / home / ojn

Then clone openjabnab:

    git clone https://github.com/OpenJabNab/OpenJabNab.git
    chown -R ojn: ojn / home / ojn / OpenJabNab /
    chmod 0777 / home / ojn / OpenJabNab / http-wrapper / ojn_admin / include

Web server configuration
============================

Do:

    cd / etc / apache2 / sites-available /
    vim ojn.conf

And add:

    <VirtualHost *: 80>
            DocumentRoot / home / ojn / OpenJabNab / http-wrapper /
            ServerName ojn.raspberry.pi
             <Directory />
                     FollowSymLinks options
                    AllowOverride None
             </ Directory>
             <Directory / home / ojn / OpenJabNab / http-wrapper />
                     Options Indexes FollowSymLinks MultiViews
                     AllowOverride all
                    Order allow, deny
                     allow from all
             </ Directory>
    </ VirtualHost>

Then activate the site:

    a2ensite ojn

Then you must allow the openjabnab server directory, do:

    vim /etc/apache2/apache2.conf

And add:

    <Directory / home / ojn />
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
    </ Directory>

Then we restart apache:

    apache2 reload service

Installation of openjabnab
=========================

Do:

    su ojn
    cd / home / ojn / OpenJabNab / server
    qmake -r
    make

> ** Note **
>
> This step can be very long (up to 45min)

Openjabnab configuration
==========================

Do:

    cp openjabnab.ini-dist bin / openjabnab.ini
    vim bin / openjabnab.ini

And change the following lines:

    StandAloneAuthBypass = true
    AllowAnonymousRegistration = true
    AllowUserManageBunny = true
    AllowUserManageZtamp = true

And replace all * my.domain.com * with * ojn.raspberry.pi *

Openjabnab web server configuration
=======================================

On your post you have to edit the file
C: \\ Windows \\ System32 \\ drivers \\ etc and add:

    192.168.0.162 ojn.raspberry.pi

Then go on:

    http: //ojn.raspberry.pi/ojn_admin/install.php

Validate it all

Starting the server
====================

That's all is ready it only remains to launch the server:

    su ojn
    cd ~ / OpenJabNab / server / bin
    ./openjabnab

Now go to:

    http: //ojn.raspberry.pi/ojn_admin/index.php

> ** Note **
>
> If all is good you should have the statistics that appear in
> bottom

Rabbit configuration
======================

To set up the rabbit it's pretty simple, you have to unplug it
then plug it back in, stay pressed. He must
normally light up in blue.

Then with your PC you should have a new wifi network
nabaztagXX, connect to it by typing 192.168.0.1.

Once on, fill in your wifi configuration and information
following:

    DHCP enabled: no
    Local Mask: 255.255.255.0
    Local gateway: 192.168.0.1 or 192.168.0.254 (depending on your network)
    DNS server: 192.168.0.162

Openjabnab server monitoring and auto start
================================================== ==

As you'll notice if you log off the server
openjabnab stops. So we have to add a little script for
monitor the server and start it automatically. Do:

    cd / home / ojn
    vim checkojn.sh

And add in:

    if [$ (ps ax | grep openjabnab | grep -v grep | wc -l) -eq 0]; Then
        su ojn; cd / home / ojn / OpenJabNab / server / bin; nohup ./openjabnab >> / dev / null 2> & 1 &
    fi

Then do:

    chmod + x checkojn.sh

Now you have to add the script at startup and a check
every 15min for example:

    crontab -e

And add:

    @reboot /home/ojn/checkojn.sh
    * / 15 * * * * /home/ojn/checkojn.sh

> ** Important **
>
> It is absolutely necessary to put it in the crontab of root, if you are
> still with the user ojn do ctrl + D

Configuring your rabbit in openjabnab
============================================

Go to :

    http: //ojn.raspberry.pi/ojn_admin/index.php

You must have :

![installation.openjabnab](../images/installation.openjabnab.PNG)

You must now create an account by clicking on create a
user:

![installation.openjabnab2](../images/installation.openjabnab2.PNG)

Fill in the requested information and log in:

![installation.openjabnab3](../images/installation.openjabnab3.PNG)

Once connected go to server:

![installation.openjabnab4](../images/installation.openjabnab4.PNG)

Then go down to find the list of connected rabbits and get back
his mac address:

![installation.openjabnab5](../images/installation.openjabnab5.PNG)

Then go to account and fill in the field name and address mac of
rabbit then validate:

![installation.openjabnab6](../images/installation.openjabnab6.PNG)

You now find on the rabbit page your rabbit, click on it
to open its configuration:

![installation.openjabnab7](../images/installation.openjabnab7.PNG)

Now you need to activate the purple API and pass it in public,
it's also here that you find the violet API key that will serve you
for Jeedom:

![installation.openjabnab8](../images/installation.openjabnab8.PNG)

Below you find the list of plugins, do not forget them
activate (TTS type or ear control):

![installation.openjabnab9](../images/installation.openjabnab9.PNG)

Configuring Jeedom
=======================

The configuration in Jeedom is quite simple, you first have to
connect in SSH to Jeedom (if you have a box Jeedom the identifiers
are in the installation doc). Then edit the file / etc / hosts

    vim / etc / hosts

And add the following line:

    192.168.0.162 ojn.raspberry.pi

Then everything happens in Jeedom, after creating your rabbit
the configuration to put:

![installation.openjabnab10](../images/installation.openjabnab10.PNG)

Here is your rabbit now has his own local burrow !!!!!

Put the TTS in local
======================

Everything is local except the TTS which passes by the site Acapela but it is
possible by editing some files to pass it locally

> ** Note **
>
> I will consider that oenjabnab is installed in
> / home / ojn / OpenJabNab and you are logged in as
> the user of openjabnab, here ojn

Creating the jeedom tts
----------------------

You need to create a jeedom folder in servver / tts:

    mkdir / home / ojn / OpenJabNab / server / tts / jeedom

Then you have to make 3 files:

-   jeedom.pro

<! - ->

    ################################################## ####################
    # Automatically generated by qmake (2.01a) Sat Jan 19 19:10:01 2008
    ################################################## ####################

    TEMPLATE = lib
    CONFIG - = debug
    CONFIG + = qt release plugin
    QT + = network xml
    QT - = mistletoe
    INCLUDEPATH + =. ../../server ../../lib
    TARGET = tts_jeedom
    DESTDIR = ../../bin/tts
    DEPENDPATH + =. ../../server ../../lib
    LIBS + = -L ../../ bin / -lcommon
    MOC_DIR = ./tmp/moc
    OBJECTS_DIR = ./tmp/obj
    win32 {
      QMAKE_CXXFLAGS_WARN_ON + = -WX
    }
    unix {
      QMAKE_LFLAGS + = -Wl, -rpath, \ '\ $$ ORIGIN \'
      QMAKE_CXXFLAGS + = -Werror
    }

    # Input
    HEADERS + = tts_jeedom.h
    SOURCES + = tts_jeedom.cpp

-   TTS \ _jeedom.h

<! - ->

    #ifndef _TTSACAPELA_H_
    #define _TTSACAPELA_H_

    #include <QHttp>
    #include <QMultiMap>
    #include <QTextStream>
    #include <QThread>
    #include "ttsinterface.h"

    class TTSJeedom: public TTSInterface
    {
      Q_OBJECT
      Q_INTERFACES (TTSInterface)

    public:
      TTSJeedom ();
      virtual ~ TTSJeedom ();
      QByteArray CreateNewSound (QString, QString, bool);

    private:
    };

    #endif

-   TTS \ _jeedom.cpp

<! - ->

    #include <QDateTime>
    #include <QUrl>
    #include <QCryptographicHash>
    #include <QMapIterator>
    #include "tts_jeedom.h"
    #include "log.h"
    #include <QNetworkReply>
    #include <QNetworkRequest>
    #include <QNetworkAccessManager>

    Q_EXPORT_PLUGIN2 (tts_jeedom, TTSJeedom)

    TTSJeedom :: TTSJeedom (): TTSInterface ("jeedom", "Jeedom")
    {
      voiceList.insert ("en", "fr");
    }

    TTSJeedom :: ~ TTSJeedom ()
    {
    }

    QByteArray TTSJeedom :: CreateNewSound (QString text, QString voice, bool forceOverwrite)
    {
      QEventLoop loop;
      if (! voiceList.contains (voice))
        voice = "fr";
      // Check (and create if needed) output folder
      QDir outputFolder = ttsFolder;
      if (! outputFolder.exists (voice))
        outputFolder.mkdir (voice);

      if (! outputFolder.cd (voice))
      {
        LogError (QString ("Cant create TTS Folder:% 1"). Arg (ttsFolder.absoluteFilePath (voice)));
        return QByteArray ();
      }

      // Compute file
      QString fileName = QCryptographicHash :: hash (text.toAscii (), QCryptographicHash :: Md5) .toHex (). Append (". Mp3");
      QString filePath = outputFolder.absoluteFilePath (fileName);

      if (! forceOverwrite && QFile :: exists (filePath))
        return ttsHTTPUrl.arg (voice, fileName) .toAscii ();

      // Fetch MP3
      QHttp http ("TODO_IP_JEEDOM");
      QObject :: connect (& http, SIGNAL (done (bool)), & loop, SLOT (quit ()));

      QByteArray ContentData;
      ContentData + = "apikey = TODO_API_JEEDOM & text =" + QUrl :: toPercentEncoding (text);

      QHttpRequestHeader Header;
      Header.addValue ("Host", "TODO_IP_JEEDOM");

      Header.setContentLength (ContentData.length ());
      Header.setRequest ("GET", "/core/api/tts.php?apikey=TODO_API_JEEDOM&text="+QUrl::toPercentEncoding(text), 1, 1);

      http.request (Header, ContentData);
      loop.exec ();

      QFile file (filePath);
      if (! file.open (QIODevice :: WriteOnly))
      {
        LogError ("Can not open sound file for writing:" + filePath);
        return QByteArray ();
      }
      file.write (http.readAll ());
      file.close ();
      return ttsHTTPUrl.arg (voice, fileName) .toAscii ();
    }

> ** Note **
>
> Do not forget to replace TODO

Then you have to activate the jeedom tts by modifying the file
/home/ojn/OpenJabNab/server/tts/tts.pro by adding jeedom to SUBDIRS:

    TEMPLATE = subdirs
    SUBDIRS = acapela google jeedom

recompilation
-------------

    cd / home / ojn / OpenJabNab / server
    qmake -r
    make

Modification of the service of tts
------------------------------

You have to edit the file /home/ojn/OpenJabNab/server/bin/openjabnab.ini
and change:

    Acapela TTS =

By

    TTS = jeedom

Revival of openjabnab
--------------------

The easiest way here is to restart the machine to restart openjabnab
