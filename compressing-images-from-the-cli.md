---
description: Easy script using imagemagick to compress images from the terminal
---

# Compressing images from the CLI

### Steps

* Move all your image files to a folder
* calculate it's folder size using `du`
* create a copy of the folder.
* install imagemagick

`brew install imagemagick`

* in this folder, perform the following actions:

```bash
mkdir compressed
for i in *; do convert $i -resize 40% compressed/$i; done;
```

The above will compress all images by making the size 40% of the initial size and will move it to a folder called `compressed`

* Check folder structure again using `du`
* if it's still too much, repeat again but this time reduce quality to even lower

