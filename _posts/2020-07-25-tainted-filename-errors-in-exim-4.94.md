---
title:  "Tainted filename errors in Exim 4.94"
categories:
  - sysadmin
tags:
  - exim
last_modified_at: 2020-07-25T00:16:29+02:00
toc: true
---

After upgrading Exim to 4.94.1 the logs start showing errors regarding tainted filenames and
e-mails are being temporarily rejected.

If the error is "**taint mismatch**", then that's an exim bug. Otherwise, it is a configuration problem:
https://bugs.exim.org/show_bug.cgi?id=2587

[Upgrading notes](https://git.exim.org/exim.git/blob/HEAD:/src/README.UPDATING)

>Exim version 4.94
>-----------------
>
>Some Transports now refuse to use tainted data in constructing their delivery
>location; this WILL BREAK configurations which are not updated accordingly.
>In particular: any Transport use of $local_part which has been relying upon
>check_local_user far away in the Router to make it safe, should be updated to
>replace $local_part with $local_part_data.

After a proper lookup the untainted data is in `$local_part_data`.  
The `tainted` attribute is sticky, so just copying the content does not remove that attribute.

## Tainted not permitted

One of the errors that can be encountered is:

`Tainted '/home/mail/user@example.com' (file or directory name for transport_test transport) not permitted`


The problem there is that the filename uses tainted data from both `$local_part` and `$domain`.
The Exim config might contain something like this that has been working in Exim 4.93.x:

```
# router
test_router:
  driver = accept
  domains = !+local_domains
  transport = test_transport

# transport
test_transport:
  driver = appendfile
  maildir_format
  directory = /home/mail/${local_part}@${domain}
```

In order to fix that issue, we would replace router and transport (if that's the case) for something like this:
```
# router
test_router:
  driver = accept
  domains = lsearch;/etc/exim/domainlist
  local_parts = lsearch;/etc/exim/domainlist/$domain_data
  transport = test_transport

# transport
test_transport:
  driver = appendfile
  maildir_format
  directory = /home/mail/${local_part_data}@${domain_data}
```

## Tainted filename for search

That's another possible error that might be encountered after upgrading Exim. This is an example of what can be found in the logs:

`Tainted filename for search: '/etc/exim/aliases/mx.example.com' temporarily rejected RCPT <test@example.com>: failed to expand "${if exists{/etc/exim/aliases/$domain}{${lookup{$local_part}lsearch{/etc/exim/aliases/$domain}}}}": NULL`

We have a list lookup: "`domains = `"

`$domain` expands to tainted data. If we concatenate `$domain` to anything, the result will be a tainted string.  
In this case: `/etc/exim/aliases/$domain` is tainted and we may not use that as a filename for a lookup.

We have to use a detainted value rather than `$domain` in `$domain_data`, but we need to look at the actual
definition of our local_domains list and work out what the lookup against it will do.

```
system_aliases:
  driver = redirect
  data = ${if exists{/etc/exim/aliases/$domain}{${lookup{$local_part}lsearch{/etc/exim/aliases/$domain}}}}
  domains = mx.example.com
```

This is a possible solution using untainted `$local_part_data` and `$domain_data`:
```
system_aliases:
  driver = redirect
  data = $local_part_data
  domains = dsearch,ret=full;//etc/exim/aliases
  local_parts = lsearch;$domain_data
```