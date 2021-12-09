<!-- markdownlint-configure-file { "MD004": { "style": "consistent" } } -->
<!-- markdownlint-disable MD033 -->
#

<p align="center">
    <a href="https://pi-hole.net/">
        <img src="https://pi-hole.github.io/graphics/Vortex/Vortex_with_Wordmark.svg" width="150" height="260" alt="Pi-hole">
    </a>
    <br>
    <strong>Network-wide ad blocking via Tinycore</strong>
</p>
<!-- markdownlint-enable MD033 -->

The Tiny-hole is a Tinycore port of Pi-hole®, a [DNS sinkhole](https://en.wikipedia.org/wiki/DNS_Sinkhole) that protects your devices from unwanted content without installing any client-side software.

- **(Somewhat)Easy-to-install**: Simply install using tce-load on the TinyCore Operating System.  
- **Resolute**: content is blocked in _non-browser locations_, such as ad-laden mobile apps and smart TVs
- **Responsive**: seamlessly speeds up the feel of everyday browsing by caching DNS queries
- **Lightweight**: runs smoothly with [minimal hardware and software requirements](https://docs.pi-hole.net/main/prerequisites/)
- **Robust**: a command line interface that is quality assured for interoperability
- **Insightful**: a beautiful responsive Web Interface dashboard to view and control your Pi-hole
- **Versatile**: can optionally function as a [DHCP server](https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026), ensuring *all* your devices are protected automatically
- **Scalable**: [capable of handling hundreds of millions of queries](https://pi-hole.net/2017/05/24/how-much-traffic-can-pi-hole-handle/) when installed on server-grade hardware
- **Modern**: blocks ads over both IPv4 and IPv6
- **Free**: open source software that helps ensure _you_ are the sole person in control of your privacy

-----

## One-Step Automated Install

Those who want to get started quickly and conveniently may install Pi-hole using the following command:

### `tce-load -i /location/of/pihole.tcz`
or
### `tce-load -wi pihole`
once it gets added to the Tinycore repository

## Alternative Install Methods

After downloading the tcz extension file, add the `pihole.tcz, pihole.tcz.dep,` and `pihole.tcz.md5.txt` to the `optional/` directory and append `pihole.tcz` to the `onboot.lst` file to autoload the extension on startup.

## [Post-install: Make your network take advantage of Pi-hole](https://docs.pi-hole.net/main/post-install/)

Once the installer has been run, you will need to [configure your router to have **DHCP clients use Pi-hole as their DNS server**](https://discourse.pi-hole.net/t/how-do-i-configure-my-devices-to-use-pi-hole-as-their-dns-server/245) which ensures that all devices connecting to your network will have content blocked without any further intervention.

If your router does not support setting the DNS server, you can [use Pi-hole's built-in DHCP server](https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026); be sure to disable DHCP on your router first (if it has that feature available).

As a last resort, you can manually set each device to use Pi-hole as their DNS server.

### IMPORTANT NOTE

The default .tcz extension comes with google's dns and default settings. You will need to configure your install in the `/etc/pihole/setupVars.conf` configuration and change the DNS settings, as well as change the nameserver values in `/etc/resolv.conf` to your choice of DNS servers. 
Afterwards, to retain persistence among reboots, add `etc/pihole` in `/opt/.filetool.lst` and run a backup.

_Note: As of pihole-FTL version 5.11, there is a bug(?) in the FTL engine that, should the resolv.conf file be edited while the engine is running (including by udhcpd renewing an IP) the pihole software will begin rejecting DNS requests._ As the nameservers will be persistent, it is safe to edit the udhcpc script `/usr/share/udhcpc/default.script` to not change the resolv.conf file. Change the following line in the script:

`echo -n > $RESOLV_CONF`

to:

``` \
dns="" \
#echo -n > $RESOLV_CONF \
```
This sets the DHCP variable `dns` to be blank, disallowing any processing of the nameservers later in the script and writing them to the file.
The hash `#` comments out the echo command which would wipe the resolv.conf file. From this point on, the resolv.conf file will no longer be changed every lease renw.

-----

## Pi-hole is free but powered by your support

Although Tiny-hole is derivative of the Pi-hole software and is developed and maintained parrallel to Pi-hole, the original Pi-hole software is an important contribution to the FOSS community and it would be beneficial to cover their costs of maintaining and developing the original Pi-hole software. As stated in the original README: 

>There are many reoccurring costs involved with maintaining free, open source, and privacy-respecting software; expenses which [our volunteer developers](https://github.com/orgs/pi-hole/people) pitch in to cover out-of-pocket. This is just one example of how strongly we feel about our software and the importance of keeping it maintained.

>Make no mistake: **your support is absolutely vital to help keep us innovating!** 

### [Donations](https://pi-hole.net/donate)

Donating using Pi-hole's Sponsor Button is **extremely helpful** in offsetting a portion of their monthly expenses:

### Alternative support

If you'd rather not donate (_which is okay!_), there are other ways you can help support Pi-hole:

- [GitHub Sponsors](https://github.com/sponsors/pi-hole/)
- [Patreon](https://patreon.com/pihole)
- [Hetzner Cloud](https://hetzner.cloud/?ref=7aceisRX3AzA) _affiliate link_
- [Digital Ocean](https://www.digitalocean.com/?refcode=344d234950e1) _affiliate link_
- [Stickermule](https://www.stickermule.com/unlock?ref_id=9127301701&utm_medium=link&utm_source=invite) _earn a $10 credit after your first purchase_
- [Amazon US](http://www.amazon.com/exec/obidos/redirect-home/pihole09-20) _affiliate link_
- Spreading the word about our software and how you have benefited from it

### Contributing via GitHub

We welcome _everyone_ to contribute to issue reports, suggest new features, and create pull requests.

If you have something to add - anything from a typo through to a whole new feature, we're happy to check it out! Just make sure to fill out our template when submitting your request; the questions it asks will help the volunteers quickly understand what you're aiming to achieve.

You'll find that the [install script](https://github.com/pi-hole/pi-hole/blob/master/automated%20install/basic-install.sh) and the [debug script](https://github.com/pi-hole/pi-hole/blob/master/advanced/Scripts/piholeDebug.sh) have an abundance of comments, which will help you better understand how Pi-hole works. They're also a valuable resource to those who want to learn how to write scripts or code a program! We encourage anyone who likes to tinker to read through it and submit a pull request for us to review.

-----


## Breakdown of Features

### [Faster-than-light Engine](https://github.com/pi-hole/ftl)

[FTLDNS](https://github.com/pi-hole/ftl) is a lightweight, purpose-built daemon used to provide statistics needed for the Web Interface, and its API can be easily integrated into your own projects. As the name implies, FTLDNS does this all *very quickly*!

Some of the statistics you can integrate include:

- Total number of domains being blocked
- Total number of DNS queries today
- Total number of ads blocked today
- Percentage of ads blocked
- Unique domains
- Queries forwarded (to your chosen upstream DNS server)
- Queries cached
- Unique clients

Access the API via [`telnet`](https://github.com/pi-hole/FTL), the Web (`admin/api.php`) and Command Line (`pihole -c -j`). You can find out [more details over here](https://discourse.pi-hole.net/t/pi-hole-api/1863).

_Note: Web interface is not available for the Tiny-hole port_

### The Command Line Interface

The [pihole](https://docs.pi-hole.net/core/pihole-command/) command has all the functionality necessary to fully administer the Pi-hole, without the need of the Web Interface. It's fast, user-friendly, and auditable by anyone with an understanding of `bash`.

Some notable features include:

- [Whitelisting, Blacklisting, and Regex](https://docs.pi-hole.net/core/pihole-command/#whitelisting-blacklisting-and-regex)
- [Debugging utility](https://docs.pi-hole.net/core/pihole-command/#debugger)
- [Viewing the live log file](https://docs.pi-hole.net/core/pihole-command/#tail)
- [Updating Ad Lists](https://docs.pi-hole.net/core/pihole-command/#gravity)
- [Querying Ad Lists for blocked domains](https://docs.pi-hole.net/core/pihole-command/#query)
- [Enabling and Disabling Pi-hole](https://docs.pi-hole.net/core/pihole-command/#enable-disable)
- ... and *many* more!

You can read our [Core Feature Breakdown](https://docs.pi-hole.net/core/pihole-command/#pi-hole-core) for more information.
