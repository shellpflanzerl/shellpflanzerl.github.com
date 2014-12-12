Title: Upgrade to Fedora 21
Date: 2014-12-12 07:57
Tags: Linux
Authors: Martin Zehetmayer

Most of you already read it - Fedora 21 is out in the wild and ready to be installed. 
The main different are that there are flavours of Fedora: Workstation, Server and Cloud. 
They are different in the number of packages installed - for example on the Workstation
installation there will me (of course) a graphical interface which will not appear on 
the server and the much smaller cloud flavour and in the default configuration of the 
systems (firewalld rules, for example)

Coming from Fedora 20 I gave it a try on day 1 of the release to upgrade my Notebook: 

    :::console
    # fedup --network 21 --product workstation 
    [... download ... update kernel ... ]
    # reboot

Surprisingly the upgrade runs without errors. Being a burnt child from the last updates
I was sceptical if this works out - but hey, no risk no fun :-) 

After that I was brave enough to update my NAS too: 

    :::console
    # fedup --network 21 --product server 
    [... download ... update kernel ... ]
    # reboot

Again it worked seamlessly.


So all I can say is: Good work, Fedoraproject! Well done! 
