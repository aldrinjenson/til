---
description: Easy way to drag and drop from terminal to anywhere else in Mac OS
---

# Adding Drag drop from terminal in Mac OS

Use this cool tool:

{% embed url="https://github.com/Wevah/dragterm" %}

### Steps to install:

1. git clone the above repo&#x20;
2. cd dragterm
3. `brew install cocoapods`&#x20;
4. `g++ DTDraggingSourceView.m main.m -framework Cocoa -o drag`
5. Add the folder to PATH

`export PATH="<your drag term folder path>:$PATH"`

### Usage:

`drag <filename>`

### Bonus

If you are using `ranger` or some other CLI based file manager, you can easily add support to dragging from terminal using `dragterm` with a key-map as follows:

```bash
map <C-d> shell drag %p
```

Paste the above to `.config/ranger/rc.conf` to map ctrl+d to drag a file from ranger&#x20;

