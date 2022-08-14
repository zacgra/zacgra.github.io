---
title: "Authentication - Part 3: The Implementation"
author: zacgra
layout: post
categories: Code
tags: [authentication, security]
---

> Disclaimer: This is part of a series I'm writing based on the content found in App Academy Open as well as my own research. The writings here are purely an academic exercise and not intended as advice on how to implement security.

Now that we have an overview of how this will work, let's start building our implementation. We will be using Ruby on Rail's BCrypt implementation found here: https://github.com/bcrypt-ruby/bcrypt-ruby. This repo contains the same implementation we will be using here.

## The User Model

Previously, we said that the user would create an account, and while doing so, they would give us their password. So let's start in the User model.

```rb
class User < ApplicationRecord
    validates :password, length: {minimum: 6}, allow_nil: true

    attr_reader :password

    def password=(password)
        @password = password
        self.password_digest = BCrypt::Password.create(password)
    end

    def self.find_by_credentials(email, password)
        user = User.find_by(email: email)
        return nil if user.nil?
        user.is_password?(password) ? user : nil
    end

    def is_password?
        BCrypt::Password.new(self.password_digest).is_password?(password)
    end
end

```

`password=(password)`
This stores the password argument as a User instance variable (but doesn't store it in the db!). It also sets the password_digest as the hashed password using BCrypt.

`User::find_by_credentials(email, password)`
This allows us to look up a user by their email and if that user exists in the db, we can compare the db user password with the logging-in user's password. If they are the same, we return the user, otherwise we return `nil`.

`is_password?`
This one is a bit tricky. On the surface, it is taking in a digest, creating a new BCrypt::Password object, then comparing the password to the password argument variable. But how can it create the password from the digest? Didn't we say that the hashing function was one-way? To understand what is going on, we can take a look at the source code on the GitHub repo we mentioned earlier. Going in to `lib/bcrypt/password.rb`, we see on line 76:

```rb
    def ==(secret)
      super(BCrypt::Engine.hash_secret(secret, @salt))
    end
    alias_method :is_password?, :==
```

So what we are looking at is a method overriding the equality operator, along with an alias of that override to `is_password?`. So now, what is `hash_secret`? Jumping over to `lib/bcrypt/engine.rb`, we see on line 55 the `hash_secret` method. Rather than going line by line through this, we can note that at the end of all of this, hash_secret calls `_bc_crypt(secret, salt)` which it says earlier in `engine.rb` is a C-level routine.

```rb
    # Given a secret and a valid salt (see BCrypt::Engine.generate_salt) calculates
    # a bcrypt() password hash. Secrets longer than 72 bytes are truncated.
    def self.hash_secret(secret, salt, _ = nil)
      unless _.nil?
        warn "[DEPRECATION] Passing the third argument to " \
             "`BCrypt::Engine.hash_secret` is deprecated. " \
             "Please do not pass the third argument which " \
             "is currently not used."
      end

      if valid_secret?(secret)
        if valid_salt?(salt)
          if RUBY_PLATFORM == "java"
            Java.bcrypt_jruby.BCrypt.hashpw(secret.to_s.to_java_bytes, salt.to_s)
          else
            secret = secret.to_s
            secret = secret.byteslice(0, MAX_SECRET_BYTESIZE) if secret && secret.bytesize > MAX_SECRET_BYTESIZE
            __bc_crypt(secret, salt)
          end
        else
          raise Errors::InvalidSalt.new("invalid salt")
        end
      else
        raise Errors::InvalidSecret.new("invalid secret")
      end
    end

```
