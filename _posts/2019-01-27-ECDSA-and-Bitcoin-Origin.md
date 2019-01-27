---
layout: post
title:  "ECDSA and Bitcoin: Origin"
categories: blog update
author: Jun Wang
---

[Collision-Resistant Elliptic Curve Hash Functions][patent] were invented by Brown et al, usually mentioned or refered as Standards for Efficient Cryptogrpahy(SEC) or Standards for Efficient Cryptography Group ([SECG][secg]).  
In [Elliptic Curve Cryptography: page 6-7][v1-1], there are some details about epllitic curves.
![ec1](/Blog/assets/img/ec1.png)
![ec2](/Blog/assets/img/ec2.png)


----
****  

`secp256k1`, which Bitcoin used, can be found in [Recommended Elliptic Curve Domain Parameters: page 15][v1-2].
![secp256k1](/Blog/assets/img/secp256k1.png)

----
****


[patent]: https://patentimages.storage.googleapis.com/ed/69/90/4e2edac247a783/US8891756.pdf
[secg]: http://www.secg.org/
[v1-2]: http://www.secg.org/SEC2-Ver-1.0.pdf
[v1-1]: http://www.secg.org/SEC1-Ver-1.0.pdf
