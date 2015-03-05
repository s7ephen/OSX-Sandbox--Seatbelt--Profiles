# Seatbelt Profiles


## Description

This is a collection of profiles for the OSX Application  Sandbox called Seatbelt. For some background information on how this whole thing works, check out [Dion Blazakis's Blackhat Talk](http://docs.google.com/viewer?aa=v&q=cache:3ZoPr0y_aeMJ:https://media.blackhat.com/bh-dc-11/Blazakis/BlackHat_DC_2011_Blazakis_Apple_Sandbox-wp.pdf).

My hopes are that people will start to make their own by maybe forking this repository and adding new rules and profiles to it and then committing back to here with pull-requests.

## Basic Usage

To get things rolling immediately you should just be able to run the corresponding shell scripts. I have taken care to make them portable. Dropping to a terminal (for example) and running:
```
    $ ./safari.sh 
```
this should fire up a "fresh" Safari.  To learn more about the sandbox profile syntax you can't easily [google for them](http://www.google.com/search?q=version+1+filetype:sb). You can however learn a bit by looking at the ones that ship with OS X by default by dropping to a terminal and looking in ```/usr/share/sandbox```:
```
    stephens-computer:sandbox_profiles s7$ cd /usr/share/sandbox/
    stephens-computer:sandbox s7$ ls
    awacsd.sb                    mds.sb                       sshd.sb
    bsd.sb                       mdworker.sb                  syslogd.sb
    cvmsCompAgent.sb             named.sb                     xgridagentd.sb
    cvmsServer.sb                ntpd.sb                      xgridagentd_task_nobody.sb
    fontmover.sb                 portmap.sb                   xgridagentd_task_somebody.sb
    kadmind.sb                   preview.sb                   xgridcontrollerd.sb
    krb5kdc.sb                   quicklookd-job-creation.sb
    mDNSResponder.sb             quicklookd.sb
    stephens-computer:sandbox s7$
```

You can also glean a bit more by checking the manpages for ```sandbox-exec```, and ```sandbox_init```.


