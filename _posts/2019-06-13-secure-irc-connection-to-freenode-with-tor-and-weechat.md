---
title:  "Secure IRC connection to Freenode with Tor and WeeChat"
toc: true
categories:
  - security
tags:
  - irc
  - debian
  - tor
  - weechat
  - privacy
date: 2019-06-13T08:49:15+02:00
last_modified_at: 2019-06-13T08:49:15+02:00
---

Let's say we want to join a hax0rs [irc](https://en.wikipedia.org/wiki/Internet_Relay_Chat) channel to chat with some [black hat](https://en.wikipedia.org/wiki/Black_hat_(computer_security)) folks. We wouldn't want to expose our ip address, right? We are going to do it using [Tor](https://www.torproject.org/) to access [freenode](https://freenode.net/) on the irc client [WeeChat](https://weechat.org/).

Pretty much the same would work if we wanted to connect to other IRC networks that have an onion address, like [AlphaChat](https://www.alphachat.net/tor.xhtml).

We want root.
```bash
sudo -i
```

## Tor

No further configuration is needed here, just install Tor service and enable it:

```bash
apt install tor
systemctl enable tor.service
systemctl start tor.service
```

We will use the SOCKS proxy to connect to Tor. Tor listens for `SOCKS` connections on port `9050` and Tor Browser on `9150`.

In case some troubleshooting is needed, the files that might help here are:

`/var/log/tor/log`  
`/etc/tor/torsocks.conf`

Also confirm that Tor is listening on `9050`.

```bash
# ss -ltap | grep 9050

LISTEN  0  0  127.0.0.1:9050  *:*  users:(("tor",pid=2177,fd=6))
```

### Test it

```bash
curl --socks5 localhost:9050 \
     --socks5-hostname localhost:9050 \
     -s https://check.torproject.org/ \
     | cat | grep -m 1 Congratulations | xargs
```

The output should be something like this:

`Congratulations. This browser is configured to use Tor.`
{: .notice--success}

That's the official documentation for Debian: [Tor documentation](https://2019.www.torproject.org/docs/debian.html.en).


## WeeChat

WeeChat is the IRC client we will use to chat with the pals in freenode. There are other options out there. It's just a matter of taste.

```bash
apt-get install weechat
```

And we start WeeChat by running:

```bash
weechat-curses
```

### Improving privacy

Add somes settings bellow to WeeChat.

```bash
/set irc.server_default.msg_part ""
/set irc.server_default.msg_quit ""
/set irc.ctcp.clientinfo ""
/set irc.ctcp.finger ""
/set irc.ctcp.source ""
/set irc.ctcp.time ""
/set irc.ctcp.userinfo ""
/set irc.ctcp.version ""
/set irc.ctcp.ping ""
/plugin unload xfer
/set weechat.plugin.autoload "*,!xfer"
```

## Freenode

Freenode like other IRC networks has some requirements when it comes to connecting via Tor, like registering a nickname and connecting using SASL.

To connect to Freenode we will use the following hidden service as the server address provided by Freenode:

`ajnvpgl6prmkb7yktvue6im5wiedlz2w32uhcwaamdiecdrfpwwgnlqd.onion`

This is the link to the Tor freenode's docs:
  * [Accessing freenode via TOR](https://freenode.net/kb/answer/chat#accessing-freenode-via-tor)

>The hidden service requires SASL authentication. In addition, due to the abuse that led Tor access to be disabled in the past, we have unfortunately had to add another couple of restrictions.

We must log in using `SASL EXTERNAL` or `ECDSA-NIST256P-CHALLENGE`:
  * If you log out while connected via Tor, you will not be able to log in without reconnecting.
  * It is recommended to use SASL EXTERNAL.

Connecting using SASL EXTERNAL requires connecting using SSL encryption.

The SSL certificates don't match the hidden services, therefore is not necessary to do any verification on the certs.

If you don't want to disable the verification in WeeChat, you can map the freenode address to the onion hidden service. Add this line to the `/etc/tor/torrc` file:

```bash
# torrc snippet:
MapAddress zettel.freenode.net ajnvpgl6prmkb7yktvue6im5wiedlz2w32uhcwaamdiecdrfpwwgnlqd.onion
```

Don't forget to reload tor service:
```bash
systemctl reload tor.service
```

### <a name="register-on-freenode"></a>Registering on Freenode

On WeeChat:

```bash
/server add freenode chat.freenode.net/6667 -autoconnect
/set irc.server.freenode.nicks mycoolnickname
/connect freenode
```

We have to create an account, this is a requirement to use TOR:
```bash
/msg NickServ REGISTER mypassword mycoolemail@example.com
```

```bash
/msg NickServ SET PRIVATE ON
```

Confirm registration after getting the mail with the code:
```bash
/msg NickServ VERIFY REGISTER mycoolnickname code1235678
```

To identify ourselves:
```bash
/msg NickServ IDENTIFY mypassword
```

### Enabling SASL EXTERNAL

```bash
mkdir ~/.weechat/certs
cd ~/.weechat/certs
```

```bash
openssl req -x509 -new -newkey rsa:4096 -sha256 -days 1000 -nodes -out freenode.pem -keyout freenode.pem
```

Find sha1sum fingerprint:
```bash
openssl x509 -in freenode.pem -outform der | sha1sum -b | cut -d' ' -f1
```

And add the fingerprint on WeeChat, eg: fingerprint `0123456789abcdefghijklmnopqrst1234567890`

```bash
/msg nickserv cert add 0123456789abcdefghijklmnopqrst1234567890
/set irc.server.freenode.ssl_cert "%h/certs/freenode.pem"
/set irc.server.freenode.sasl_mechanism external
/set irc.server.freenode.ssl on
/set irc.server.freenode.addresses "chat.freenode.net/6697"
/reconnect freenode
```

### Connection over TOR

Now that we have our nickname and our certificate we can connect to freenode via tor. Below are the steps needed but feel free to check out the official information:

  * https://freenode.net/kb/answer/chat#accessing-freenode-via-tor

Add the Onion adress and the proxy:

```bash
/set irc.server.freenode.addresses "ajnvpgl6prmkb7yktvue6im5wiedlz2w32uhcwaamdiecdrfpwwgnlqd.onion/7000"
/proxy add tor socks5 127.0.0.1 9050
/set irc.server.freenode.proxy "tor"
```

We disable ssl_verify, which doesn’t work with TOR.
```bash
/set irc.server.freenode.ssl_verify off
/reconnect freenode
```

### Shortcuts WeeChat

This section is just for me to remember some WeeChat shortcuts. It's part of the official documentation of WeeChat that you can find [here](https://weechat.org/files/doc/stable/weechat_quickstart.en.html).

#### join/part channels

Join a channel:
```bash
/join #channel
```

Part a channel (keeping the buffer open):
```bash
/part [quit message]
```

Close a server, channel or private buffer (/close is an alias for /buffer close):
```bash
/close
```

Closing the server buffer will close all channel/private buffers.

Disconnect from server, on the server buffer:
```bash
/disconnect
```

#### private messages

Open a buffer and send a message to another user (nick foo):

```bash
/query foo this is a message
```

Close the private buffer:
```bash
/close
```

#### buffer/window management

A buffer is a component linked to a plugin with a number, a category, and a name. A buffer contains the data displayed on the screen.

A window is a view on a buffer. By default there’s only one window displaying one buffer. If you split the screen, you will see many windows with many buffers at same time.

Commands to manage buffers and windows:
```bash
/buffer
/window
```

For example, to vertically split your screen into a small window (1/3 width), and a large window (2/3), use command:

```bash
/window splitv 33
```

To remove the split:
```bash
/window merge
```

#### Key bindings

WeeChat uses many keys by default. All these keys are in the documentation, but you should know at least some vital keys:

**Alt+← / Alt+→ or F5 / F6**: switch to previous/next buffer

**F1 / F2**: scroll bar with list of buffers ("buflist")

**F7 / F8**: switch to previous/next window (when screen is split)

**F9 / F10**: scroll title bar

**F11 / F12**: scroll nicklist

**Tab**: complete text in input bar, like in your shell

**PgUp / PgDn**: scroll text in current buffer

**Alt+a**: jump to buffer with activity (in hotlist)

According to your keyboard and/or your needs, you can rebind any key to a command with the /key command. A useful key is Alt+k to find key codes.

For example, to bind Alt+! to the command /buffer close:
```bash
/key bind (press alt-k) (press alt-!) /buffer close
```

You’ll have a command line like:
```bash
/key bind meta-! /buffer close
```

To remove key:
```bash
/key unbind meta-!
```