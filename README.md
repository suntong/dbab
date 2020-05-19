# dbab(8) -- dnsmasq based ad blocking

## SYNOPSIS

    # start dbab-svr server
	/etc/init.d/dbab start

    # stop dbab-svr server
	/etc/init.d/dbab stop

    # get/update ad blocking list
	/usr/sbin/dbab-get-list

	# add your own to the ad blocking list
	/usr/sbin/dbab-add-list


## DESCRIPTION

`dbab` provides a total solution for SOHO service environment, smoothly integrates `DHCP`, `DNS`, local caching and Ad blocking into harmony operation.
Ad blocking is done by `DNSmasq` and `dbab-svr` the pixel server, i.e., done at the DNS level -- all requests to ads-sites are blocked right there at DNS level. No more user space extensive pattern matching necessary at all. Work for your mobile devices as well. You don't need to install anything to your mobile devices to enjoy the ad-free and speed-up browsing.


## ALTERNATIVES

People may also use browsers' `adblock-plus` extension to block ads, but fewer think over how it works internally. Here is an overview of Adblock Plus from a thousand mile high [1] -- whenever the browser needs to load something, the extension kicks in and do a thorough pattern matching of all known ad urls using regular expressions, then hectically replace all found ad urls with something else. This is done on every page, every load, and every component of the web page, using JavaScript. Thus it is by nature slow and CPU intensive, at least inefficient. There are other alternatives to this, e.g., `privoxy`, but the concepts are the same.

[1] http://adblockplus.org/en/faq_internal

## ADVANTAGES

Comparing to other ad-blocking efforts, `dbab` will be super light. Only a few operations are enough to determine and stop the ads. No heavy-lifting (using CPU intensive URL pattern matching) necessary. Thus it will be super light and lightning fast.

The advantages of using `dbab` are:

- **Work at the DNS level**. Leave the web pages intact, without any pattern matching, string substitution, and/or html elements replacing.
- **Work for your mobile devices as well**. Were you previously in the dilemma of choosing ads free or slow response for your mobile devices (iphone, ipad, etc)? Now you don't. You don't need to install any thing to your mobile devices for them to enjoy the ad-free browsing experience. Moreover, their browsing speed will increase dramatically on revisited pages/images. 
- **Serve instantly**. All ads will be replaced by a `1x1` pixel gif image served locally by the `dbab-svr` pixel server.
- **Maintenance free**. You don't need to maintain the list of ad sites yourself. The block list can be downloaded from pgl.yoyo.org periodically. If you don't like some of the entries there, you can add-to or remove-from that list easily.

## DBAB-SVR

The `dbab-svr` is a super minimal web / pixel server, it has one purpose -- serving a `1x1` pixel transparent gif file. It can optionally provide the automatic WPAD service as well if so configured. By default it listens on `localhost`, configurable from the file `/etc/dbab/dbab.addr`.

## DBAB-GET-LIST

The `dbab-get-list` is used to get `dnsmasq` blocking list from pgl.yoyo.org to be used by `DNSmasq`. The result is stored as `/etc/dnsmasq.d/dbab-map.adblock.conf`.

You can run it once, or put it in a `cron` job so as to update the block list periodically. E.g., to update on a weekly basis:

    ln -s /usr/sbin/dbab-get-list /etc/cron.weekly/

It is safe to do so, even if the machine might be offline when the `cron` job is triggered. The existing file will be intact if download failed.

## DBAB-ADD-LIST

You can use `dbab-add-list` to add your own entries to `dnsmasq` blocking list, if the list from pgl.yoyo.org is not sufficient for you. The result is stored as `/etc/dnsmasq.d/dbab-map.trashsites.conf`.

## DBAB-CHK-LIST

The `dbab-chk-list` can help you to check if your own list is already covered by pgl.yoyo.org.

## DHCP-ADD-WPAD

The `dhcp-add-wpad` will take the content in `/etc/dbab/dbab.addr` as the IP of the host of the  `dhcp` server, the squid caching server, then enable the automatic WPAD service within the system, with the help of the `DNS` and `DHCP` server.

## FILES 

* `/etc/dbab/dbab.addr`  
  The IP address that `dbab-svr` listens on. Defaults to `localhost`.
  
* `/etc/dbab/dbab.list-`  
  The entries you want to filter out from the pgl.yoyo.org lists. List sites you still wish to visit there. 

* `/etc/dbab/dbab.list+`  
  The entries you want to add to blocking list on top of the pgl.yoyo.org list, used by `dbab-add-list`. 

* `/etc/dnsmasq.d/dbab-map.adblock.conf`  
  The file which `dbab-get-list` updates.

* `/etc/dnsmasq.d/dbab-map.trashsites.conf`  
  The file which `dbab-add-list` updates.

* `/usr/share/doc/dbab/dbab.md`  
  The more detailed introduction and installation guild.


## AUTHOR(S)

Copyright: 2013~2020 Tong SUN  
![suntong001 from users.sourceforge.net](https://img.shields.io/badge/suntong001-%40users.sourceforge.net-lightgrey.svg "suntong001 from users.sourceforge.net")

The pixelserv was originally downloaded from  
 http://proxytunnel.sourceforge.net/files/pixelserv.pl.txt  
Wrote by Piet Wintjens, with BSD (no advertising clause) license.
