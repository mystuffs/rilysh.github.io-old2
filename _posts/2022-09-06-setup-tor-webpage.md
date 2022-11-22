---
layout: post
title: Setup a Tor webpage
subtitle: Run your own onion site
tags: [public]
---

## What is Tor?
Tor, full form The Onion Router is free and open-source software to do anonymous communication. Tor basically directs your Internet traffic through free relays, based worldwide and maintained
by volunteers. Using Tor makes it more difficult for your ISP (Internet Service Provider) to know about
your Internet activity, it also hides your IP address by assigning a random IP based on Tor main guard
and exit node.

To learn more about Tor and Tor Browser (which helps you to access the darknet), [click here](https://en.wikipedia.org/wiki/Tor_(network)).

## Requirements
A basic computer with GNU/Linux running. It would be great if you have a Rasberry pi.\
Note: You can other operating systems too, for example, FreeBSD, OpenBSD, or even macOS, but here I'll show you how to set up all these stuff inside a GNU/Linux machine.

## Installation
To know how to install Tor, please visit [here](https://community.torproject.org/onion-services/setup/install/).\
For more info about installation in Fedora and RedHat, please visit [here](https://support.torproject.org/rpm/).

After the installation process is done, enable Tor as a service.

## Configure Tor
Now open your `torrc` file, on GNU/Linux it will be located at `/etc/tor/torrc` in edit mode, and uncomment these two lines, provided below.
```
HiddenServiceDir
HiddenServicePort
```
Change the IP port to 1337 or whatever you'd like, and make sure the port is open for connection.
You can change the IP address as well to another proxy or system IP. It's recommended to use UNIX sockets over TLS
especially if you want to hide Tor service from your ISP.

Example changes should look like this
```
HiddenServiceDir /var/lib/tor/hidden_service
HiddenServicePort 80 127.0.0.1:1337
```

Save the `torrc` file and restart the tor service.

## Run a server
Everything is almost done, but before serving your webpage you need to set up a server. You can choose [NGINX](https://www.nginx.com/) or [Apache](https://httpd.apache.org/).\
They both require manual configuration, for more info check out their installation guides.\
[http://nginx.org/en/download.html](http://nginx.org/en/download.html)\
[https://httpd.apache.org/download.cgi](https://httpd.apache.org/download.cgi)

However, NGINX and Apache are both made for high concurrent requests and load balance. If you want something lightweight without installing any of this software you can run your own server on that port as well.
I've implemented it in Rust, you may choose other languages like Go. To obtain the source click [here](https://0.0g.gg/?7284d1b6fc12ba98#7S51aVm3n1E7kN8UN9SK7WVjKGmXT1kDMtwuPsRsmckT).

In the following code, there's a variable called `ip`. `ip` is where the page will be run (regarding you can use your router IP).
You can also provide proxy IP as well, even though I didn't yet test will it going to work or not.

For instance, we'll also create a simple HTML file.

```html
<html>
  <pre>
    Hello world!
  </pre>
</html>
```
Save the file as `index.html` (you can any other name) and locate the file path in `file` variable.

Compile the code (with release flag, `cargo build --release`) and you should have output binary located `./target/release/{based_on_dir_name}`\
Run the executable file and open `localhost:1337` in your browser, the page should be up!

<div align="center">
    <img src="https://user-images.githubusercontent.com/71683721/189203432-f6ae25dc-d860-4b6f-a993-09d6a5e12f7e.png" alt="image" />
    <p>Example</p>
</div>

## Furthur Info
This is a very simple introduction to the usage of Tor to host your site. 
If you want to learn more about the darknet and decentralized networks, documents and papers about them are worth checking.

[https://en.wikipedia.org/wiki/Tor_(network)](https://en.wikipedia.org/wiki/Tor_(network))\
[Simplified: https://simple.wikipedia.org/wiki/Tor_(software)](https://simple.wikipedia.org/wiki/Tor_(software))\
[https://en.wikipedia.org/wiki/Decentralized_web](https://en.wikipedia.org/wiki/Decentralized_web)\
[https://www.cl.cam.ac.uk/research/dtg/android/tor/](https://www.cl.cam.ac.uk/research/dtg/android/tor/)\
[https://www.itu.int/ITU-T/special-projects/ip-policy/final/IPPolicyHandbook-E.pdf](https://www.itu.int/ITU-T/special-projects/ip-policy/final/IPPolicyHandbook-E.pdf)
