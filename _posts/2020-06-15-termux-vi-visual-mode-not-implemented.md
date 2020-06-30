---
title:  "visual mode not implemented in vi using termux"
categories:
  - sysadmin
tags:
  - termux
  - vi
date: 2020-06-15T18:34:10+02:00
last_modified_at: 2020-06-15T18:35:31+02:00
---

When using vi on Termux we might have the issue that the visual mode is not implemented.

```
'v' is not implemented 
```

That probably happens because we are using the vi delivered with `busybox`.
To use the visual mode and some other vi options we install `vim`

```
pkg in vim
```

