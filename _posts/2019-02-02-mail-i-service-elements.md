---
title:  "Mail I: Agents"
toc: true
categories: 
  - sysadmin
tags:
  - mail
  - mua
  - msa
  - mta
  - mda
  - smtp
date: 2019-02-02T10:00:15+02:00
last_modified_at: 2019-02-02T18:22:36+02:00
---

## Introduction
This is a compendium of some _old_ notes, partly mine, partly from external sources.  
In the [references](#references) section there are some links to the sources that could have been identified.

## E-Mail

Electronic Mail (aka E-mail) is an internet service that allow users to send and to receive messages and files (e-mails) in a fast, low-cost, and efficient way.

We call **mail servers** the systems that provide this service either in private networks or publicly on Internet.

## Basic flow

Basic mail flow:
1. An email is written in a [**MUA**](#mua) and submitted to a mail server's MSA: port 587, which is for submitting mail to a the server, frequently (but not required to be) encrypted using STARTTLS.
2. The [**MSA**](#msa) and check for any errors before submitting the mail to the MTA port 25 (normally in the same SMTP server as the MSA).
3. The [**MTA**](#mta) checks the MX record off the recipient domain and transfers the message to another MTA. Mail is routed from server to server port 25 (MTAs) one or more times until arrives to the recipient's MTA.
4. The **MTA** hands the email off to the incoming mail server [**MDA**](#mda), which stores the email for retrieval by the receiving MUA
5. The receiving MUA requests the message from the **MDA** (usually with POP3 port 110 or IMAP port 143).
6. The message is delivered to the receiving **MUA's inbox**.

![Mail](/assets/images/smtp-transfer-steps.png)

## Service Elements

The mail servers have often to perform different tasks depending on their role, e.g.: read, answer, transfer messages, deliver messages, etc.

These are the agents in the mail flow process:

### <a name="mua"></a>Mail User Agent (MUA)

**MUA** or **Mail User Agent** is basically the **Mail client** that we install in our desktops. It allows us to read, write, answer, and dispose (for example: delete or archive) the e-mails. It also use the standard rules of composing an e-mail, including fields as **From** and others, and manage attachments MIME (Multipurpose Internet Mail Extensions).

Examples of MUA are:
* mail (for BSD and others)
* mutt
* Thunderbird Mail
* Outlook

### <a name="msa"></a>Mail Submission Agent (MSA)

**MSA** or **Mail Submission Agent** stands between the **MUA** and the **MTA** in a mail system. It acts as a sort of _receptionist_ (**port 587**) for messages coming in to a mail system from MUAs.

It does error checking and verification (such as verifying that hostnames are FQDNs, checking the legitimacy of local hostnames before appending the local domain portion, fixing headers, etc.) before passing the message off to the **MTA** for transmission.

The **MSA** and the **MTA** are usually hosted on the same SMTP server, as the MSA acts as a "front-end" of the MTA.

They also are relatively new. Prior to their existence, all this work was handled by the MTA. See [RFC 2476](https://tools.ietf.org/html/rfc6409).

Examples of MSA are:
* dovecot (see [Dovecot Submission](https://doc.dovecot.org/admin_manual/submission_server/))
* postfix

### <a name="mta"></a>Mail Transfer Agent (MTA)

The **MTA** or **Mail Transfer Agent** (also called **mail relay**) is the software that checks recipient domain's MX record to decide how to continue transferring the message (either to another MTA, or an MDA) and transfers emails from one computer to another using **SMTP** (port 25). It is used for communication between MTAs, or from an MSA to an MTA, this distinction is first made in RFC 2476.

When a recipient mailbox of a message is not hosted locally, the message is relayed, that is, forwarded to another MTA. Every time an MTA receives an email message, it adds a Received trace header field to the top of the header of the message,[4] thereby building a sequential record of MTAs handling the message. The process of choosing a target MTA for the next hop is also described in SMTP, but can usually be overridden by configuring the MTA software with specific routes.

Examples of MTA are:
* postfix
* sendmail
* qmail
* exim

### <a name="mda"></a>Mail Delivery Agent (MDA)

The **MDA** or **Mail Delivery Agent** is the incoming mail server, which stores the email as it waits for the user to accept it.

The client will connect to the **MDA** with their **MUA** to retrieve the email stored.

There are two main protocols used for retrieving email on an MDA:
* POP3 - Post Office Protocol
* IMAP - Internet Message Access Protocol

**POP3** is the older of the two, and is used for retrieving email and, in certain cases, leaving a copy of it on the server.

**IMAP**, on the other hand, is used for coordinating the status of emails (read, deleted, moved) across multiple email clients. With IMAP, a copy of every message is saved on the server, so that this synchronization task can be completed.

For this reason, incoming mail servers are called POP servers or IMAP servers, depending on which protocol is used.

To use a real-world analogy, **MTA**s act as the post office (the sorting area and mail carrier), which handle message transportation, while **MDA**s act as mailboxes, which store messages (as much as their volume will allow) until the recipients check the box. This means that it is not necessary for recipients to be connected in order for them to be sent email.

To keep everyone from checking other users' emails, **MDA** is **protected** by a user name called a **login** and by a **password**.

Retrieving mail is done using a software program called an **MUA** (Mail User Agent). When the MUA is a program installed on the user's system, it is called an email client (such as Mozilla Thunderbird, Microsoft Outlook, Eudora Mail, Incredimail or Lotus Notes).

Examples of MDA are:
* dovecot
* fetchmail
* procmail
* bin/mail, the MDA part of sendmail
* postfix LDA (local delivery agent)

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


Jump to [Mail II: Types, protocols and ports](/sysadmin/mail-ii-types-protocols-and-ports)

Jump to [Mail III: Local mail storage](/sysadmin/mail-iii-local-mail-storage)