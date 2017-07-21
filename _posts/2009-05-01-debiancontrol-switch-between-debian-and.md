---
layout: post
name: debiancontrol-switch-between-debian-and
title: ! 'debian/control: switch between debian and ubuntu format'
date: 2009-05-01 16:00:00.000000000 +08:00
tags: packaging debian ubuntu python
permalink: /2009/05/debiancontrol-switch-between-debian-and.html
---

```python
#!/usr/bin/env python                                                                   

import logging
import sys    

log = logging.getLogger(__name__)

def main():
    lines = file('debian/control').readlines()

    l1 = None
    l2 = None

    for idx, x in enumerate(lines):
        if x.startswith('Maintainer:'):
            l1 = idx                   
        elif x.startswith('XSBC-Original-Maintainer:'):
            l2 = idx

    if l1 is None:
        log.error("can't find maintainer line")
        sys.exit(1)

    isUbuntu = l2 is not None

    if isUbuntu:
        maintainer = lines[l2].lstrip('XSBC-Original-Maintainer:').strip()
    else:
        maintainer = lines[l1].lstrip('Maintainer:').strip()

    if isUbuntu:
        lines[l1] = 'Maintainer: %s\n' % maintainer
        del lines[l2]
    else:
        lines[l1] = 'Maintainer: Ubuntu MOTU Developers <ubuntu-motu@lists.ubuntu.com>\n' \
                + 'XSBC-Original-Maintainer: %s\n' % maintainer

    file('debian/control', 'w').writelines(lines)

if __name__ == '__main__':
    main()
```
