# Seatbelt Profiles


## Description

This is a collection of profiles for the OSX Application  Sandbox called Seatbelt. For some background information on how this whole thing works, check out [Dion Blazakis's Blackhat Talk](http://docs.google.com/viewer?aa=v&q=cache:3ZoPr0y_aeMJ:https://media.blackhat.com/bh-dc-11/Blazakis/BlackHat_DC_2011_Blazakis_Apple_Sandbox-wp.pdf).
[This guy](https://github.com/hellais) also recently did ["Buckle Up"](https://github.com/hellais/Buckle-Up) which may help some.

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

### Writing Sandbox Profiles The Easy Way (from ["Buckle Up"](https://github.com/hellais/Buckle-Up#how-to-write-a-sandbox-profile))

Use the example.sb sandbox file that contains in particular the line

   (trace "profile.sb")

This instructs sandbox-exec to output a profile.sb file that will contain
the raw output of what resources are being accessed during the runtime of the
target application.

You would therefore start the application with:

    sandbox-exec -f example.sb /Path/To/The/Application/

Then run sandbox-simplify on the profile.sb and pipe it to another file:

    sandbox-simplify profile.sb > simplified.sb

You can then start editing that simplified file to see what makes sense to keep,
what can be compacted more and what should be changed.

A useful vi macro to keep handly is this:

    %s/literal "\/Users\/replace_with_your_username/regex #"^\/Users\/[^\.]+/gc

This basically makes your profile work for people that don't have your same username.

### Writing Sandbox Profiles the Boring way

You want to start from a basic sandbox profile that contains the bare minimum necessary to start the application.
Something along the lines of this is a good starting point:

    (version 1)
    (debug allow)
    (allow process*)
    (deny default)

What this does it it allow processes to run and it is a whitelist based profile (i.e. the default policy is
to not allow).

The next thing that you want to do is start

    tail -f /var/log/system.log

All the denied by policy lines will end up in that file. Then start your application with your sandbox profile:

    sandbox-exec -f <sandbox_file>.sb /path/to/the/app

You will then see in the `tail -f` terminal lines containing something like:

    Dec 22 14:58:08 x sandboxd[12281] ([12280]): firefox-bin(12280) deny file-read-data /private/tmp

This is saying, for example, that firefox was denied "file-read-data" access to the file in /private/tmp.
You should then evaluate if you want to allow that or not and in the first case add the entry that allows
that in your sandbox file, like so:

    (file-read-data
        (regex "^/private/tmp")
    )

Continue iteratively until you reach a point where your application runs properly and all the error messages
are thing you don't want to happen.

Safe hacking and remember to fasten your seatbelt :)

## Resources

- [Dion Blazakis's Blackhat Talk](http://docs.google.com/viewer?aa=v&q=cache:3ZoPr0y_aeMJ:https://media.blackhat.com/bh-dc-11/Blazakis/BlackHat_DC_2011_Blazakis_Apple_Sandbox-wp.pdf)

- [Apple's Sandbox Guide](http://reverse.put.as/wp-content/uploads/2011/09/Apple-Sandbox-Guide-v1.0.pdf)

- [Chromium sandboxing](http://www.chromium.org/developers/design-documents/sandbox/osx-sandboxing-design)

- [Tech Journal's Intro to OSX Sandboxing](http://techjournal.318.com/security/a-brief-introduction-to-mac-os-x-sandbox-technology/)

- [Iron Suite](https://www.romab.com/ironsuite/)

