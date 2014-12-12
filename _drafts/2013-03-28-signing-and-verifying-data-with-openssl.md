---
layout: post
title: Signing and Verifying Data With OpenSSL
created: 1364520572
---
<h1>Introduction</h1>

In this article, I'll teach you how to manually sign and verify data using RSA keys using OpenSSL. This normally wouldn't be a big issue, but in this case, the RSA public key is published as an X.509 certificate. Manually verifying signed data using an X.509 certificate isn't well documented and this article aims to provide good documentation about the subject.

I make a few assumptions. I assume you know C and you know what OpenSSL is. I'm going to step through the code in sections then I'll conclude with all the code. The code that will be shown is a proof-of-concept and not meant to be pretty, optimized code. With that in mind, let's dig in!

<h1>Required Headers</h1>
In all of our source files, we'll be including the same headers:
[c]
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

#include <sys/types.h>
#include <sys/mman.h>
#include <sys/stat.h>
#include <openssl/ssl.h>
#include <openssl/err.h>

#include "mycrypto.h"
[/c]

myheader.h contains the following:
[c]
#if !defined(_MYCRYPTO_H)
#define _MYCRYPTO_H

unsigned char *simple_digest(unsigned char *, unsigned int, unsigned int *);

#endif
[/c]

<h1>Initializing OpenSSL</h1>
Initializing OpenSSL is easy:
[c]
OpenSSL_add_all_digests();
OpenSSL_add_all_algorithms();
OpenSSL_add_all_ciphers();
ERR_load_crypto_strings();
[/c]

Now
