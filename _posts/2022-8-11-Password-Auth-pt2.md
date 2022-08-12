---
title: "Authentication - Part 2: The Algorithm"
author: zacgra
layout: post
categories: Code
tags: [authentication, security]
---

> Disclaimer: This is part of a series I'm writing based on the content found in App Academy Open as well as my own research. The writings here are purely an academic exercise and not intended as advice on how to implement security.

### Creating An Authentication Algorithm

Using our reasoning from the previous discussion, now we can think in broad terms of how we might implement this process.

#### ** User Account Creation **

1. When a user creates an account, they give us their password.
2. Rather than storing that password, we salt that password, then hash it.
3. Next, we store the resulting digest in the database.

Note: Where do we store the salt? With a common hashing library like BCrypt, the salt is returned as a part in the string from the hash function, along with the BCrypt alogrithm, the hash cost and the hashed password itself.

#### ** User Login **

1. When a user logs in, they provide us an email or username, as well as a password.
2. We first look up the user by the provided email/username.
3. Next, we create a password digest from the password they provide us by salting then hashing the provided password.
4. Finally, we compare the new digest to the one stored with the user that we just looked up.
5. If the digests are the same, the provided password has been confirmed and the user has successfully authenticated.
