---
layout: post
title:  "ECDSA and Bitcoin II: Elliptic Curve and Private Key"
categories: blog update
author: Jun Wang
---

## I. The Order of the curve VS. the order of the base point
### 1. Double a point(Add a point to itself):

----
****  
The set of parameters Bitcoin use, is called `secp256k1`, which can be found in the first version of [Recommended Elliptic Curve Domain Parameters: page 15][v1-2].
![secp256k1](/Blog/assets/img/secp256k1.png)

----
****

[v1-2]: http://www.secg.org/SEC2-Ver-1.0.pdf
[PrimalityTestPython]: https://rosettacode.org/wiki/Miller%E2%80%93Rabin_primality_test#Python
[SchoofAlgorithm]:https://en.wikipedia.org/wiki/Schoof%27s_algorithm#The_algorithm
