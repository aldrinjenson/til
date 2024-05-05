---
description: Easy way to fetch availability of all trains at once in IRCTC page
---

# Fetching Train availability in IRCTC

### Problem

When you search for trains in [irctc](https://www.irctc.co.in) page, it never shows the seat availability directly. You'll have to manually go and click the refresh button of every listed train

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption><p>List of trains, but availability is not present</p></figcaption></figure>

### Steps to fix

1. Search for trains in [https://www.irctc.co.in](https://www.irctc.co.in)
2.  Apply filters to narrow down the list of trains

    <figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Narrow down list of trains using filters</p></figcaption></figure>


3. Paste the following script in console to get the availability status of all trains at once

```javascript
document.querySelectorAll('.fa.fa-repeat').forEach(el=>el.click())
```

4. The end. Tada!



### Note:

Ensure that you always try this out after narrowing down the list using filters. Else it'll be too many requests going on at once.
