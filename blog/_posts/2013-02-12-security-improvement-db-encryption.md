---
title: "Security improvement: database encryption"
created_at: Tue Feb 12 23:30 CET 2013
layout: post
author: Piotr Sarnacki
twitter: drogus
permalink: blog/2013-02-12-security-improvement-db-encryption
---

One of our goals for the nearest future is to improve security of Travis CI.
The first step in that direction is the database enryption, which I rolled out
over the last weekend. Keep reading if you want to know the details.

## Why do we need encryption?

Travis keeps at least a few pieces of data, which should be kept safe: we store
user's github token, we keep private key, which is used to decrypt secure data
from `.travis.yml` and, of course, we need to keep our [own tokens](http://about.travis-ci.org/blog/2013-01-28-token-token-token/),
which is used in github hooks and for API authentication.

All those fields should be kept private, but ulike with passwords, we can't
just store cryptographic hash, we need to be able to retrieve such data.

## How do we do encryption?

The columns containing critical data are set to be encrypted using AES256
encryption in CBC mode. Since the deploy only new or udpated data is encrypted,
but we will be encrypting existing database content over the next few days.

At this point the contents of encrypted columns are automatically decrypted
when they're accessed on an ActiveRecord object, but this is going to be
changed in the future. Ideally, any code that needs access to the decrypted
data should explicitly specify the need. With such setup it's much harder to leak
any confidential data, for example by outputing the object in logs or serializing
into JSON.

## What does it give us?

As you may noticed, if we encrypt and decrypt the data ourselves, we need to
make the key available in all the applications that have access to the database.
Because of that fact the encryption will not give us much if attacker
compromises one of the applications.

However, if database contents have been leaked out, database encryption would
mitigate the risk of gaining access to all the confidential information
that we keep. Furthermore, with a few tweaks in the code we can also make sure
that the decrypted data can't leak accidentally in any output.

## What's next?

The next thing on our security improvements list is to change a way secure
environments work. Currently we send the decrypted config directly to workers
which works fine, but it creates unnecassery risk of leaking the information
somewhere along the way. One solution that we discuss is to decrypt the
data only when needed at the last possible step using some kind of API,
which would also provide us information on any access to such data.
However we still need to discuss this more and hash out the details.
