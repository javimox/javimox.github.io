---
title: "Mail III: Local mail storage"
toc: true
categories: 
  - sysadmin
tags:
  - mail
  - mbox
  - maildir
date: 2019-02-04T21:05:21+02:00
last_modified_at: 2019-02-04T21:23:12+02:00
---

## Introduction

This is a compendium of some _old_ notes, partly mine, partly from external sources.  
In the [references](#references) section there are some links to the sources that could have been identified.

## Local Mail Storage

Long before Unix systems were networked to each other, users on a Unix host sent e-mail to each other on the local system.

Every Unix user account is capable of receiving e-mail, unless special steps have been taken to prevent this from happening. On most systems, root is not able to receive e-mail directly.

Mail is stored in one of four different formats:

### MBOX

The **mbox format** is the most common. In this format, all messages are stored in one gigantic file, which is usually ''/var/spool/mail/USERNAME''. Other common locations include `/var/mail/USERNAME` and `/usr/spool/mail/USERNAME`. The environment variable `$MAIL` should contain the full path to the mailbox.  

Messages inside an **mbox** are delimited by the 5-byte string "`From `" (note the trailing space) at the beginning of a line.  

That's why, when you send or receive Unix mail, you might find that your paragraphs beginning with "From " have a > symbol inserted in front of the word "From": it's an artifact of the mbox format.

### MMDF

The **MMDF format** is the same as mbox, except that messages are delimited by four `Ctrl-A` bytes (ASCII 01). The only implementation that commonly uses MMDF format is SCO Unix.

### MH

The **mh format** was the first mailbox format to store messages in individual files. This has the advantage that you can delete a message from the mailbox without having to copy the entire mailbox.

The only common use for this format is the mh family of mail programs. (The format is named after the program.) An mh format mailbox is usually kept in the user's home directory.

### Maildir

The **maildir format** is similar to the mh format, in that messages are stored in separate files. But maildir adds several extensions to this concept which make it more robust.

`Maildir` is reputed to be safe even when the mailbox is mounted over a network file system. Many different mail programs now support the maildir format, but it is not nearly as widely supported as mbox yet. Maildir format mailboxes are usually kept in the user's home directory.

Some Mail Transfer Agents (MTA) will deliver messages directly to local mailboxes by themselves. Others use a Mail Delivery Agent (MDA) to do that for them.

## <a name="references"></a>References

* [RFC 1939](http://www.faqs.org/rfcs/rfc1939.html), which describes POP3.
* [RFC 2060](http://www.faqs.org/rfcs/rfc2060.html), which describes IMAP.
* [RFC 2476](http://www.faqs.org/rfcs/rfc2476.html), which describes the role of an MSA.
* [RFC 2821](http://www.faqs.org/rfcs/rfc2821.html), which describes SMTP.
* [Sendmail](http://www.sendmail.org/), the oldest Unix MTA.
* [Qmail](http://www.qmail.org/), another MTA.
* [Postfix](http://www.postfix.org/), another MTA.
* [Exim](http://www.exim.org/), another MTA.
* [Procmail](http://www.procmail.org/), a popular MDA for use with sendmail, or for filtering messages.
* [Fetchmail](http://catb.org/~esr/fetchmail/), a POP3/IMAP client that retrieves e-mail from another host and then delivers it to SMTP (or procmail) on the localhost.
* [Teoría del correo (Spanish)](https://cursosasir.files.wordpress.com/2014/06/teoria_correo.pdf)
* [How E-Mail works](https://howto.lintel.in/how-does-email-work/)
* [An Introduction to Internet E-Mail](http://wooledge.org/~greg/mail.html)
* [Fastmail Docs: SSL, TLS, and STARTTLS](https://www.fastmail.com/help/technical/ssltlsstarttls.html)
* [Postfix Docs: TLS](http://www.postfix.org/TLS_README.html)

Jump to [Mail I: Agents](/sysadmin/mail-i-service-elements/)

Jump to [Mail II: Types, protocols and ports](/sysadmin/mail-ii-types-protocols-and-ports)