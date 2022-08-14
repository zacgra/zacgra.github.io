---
title: "Curl Cheatsheet"
author: zacgra
layout: post
categories: Code
tags: [endpoints, cli]
---

# Intro

While working as a math teacher, I used to side hustle for my school district doing lots of back-end things, like setting up API integrations. Once, while on a call with a curriculum's implementation engineer, we were setting up an API and I asked them what an endpoint would return. They responded by messaging me the endpoint "here's the endpoint to curl". Umm...what? I had no idea what they were referring to, so I said "Oh. Great!". Long pause. Are they on to me?

It turns out curl, or client URL, is a command-line tool that is designed to do a lot of things that developers lean on Postman for. More recently, I decided that it is annoying to always have to load up Postman, and it would be nice to have a cli version that you could script/pipe/redirect to your hearts content.

The rest of this is just a quick reference for me, so you might be better served by reading more at: [curl Tutorial](https://curl.se/docs/manual.html)

# HTTP requests in Curl

- GET

```sh
curl https://jsonplaceholder.typicode.com/posts

# send username and password with get request
curl http://name:passwd@machine.domain/full/path/to/file
# or break it out from the URL
curl -u name:passwd http://machine.domain/full/path/to/file
```

- POST

```sh
curl -X POST https://jsonplaceholder.typicode.com/posts \
              -H 'Content-Type: application/json' \
              -d '{"title":"foo","body":"bar","userId":1}'

# response:
# {
#   "title": "foo",
#   "body": "bar",
#   "userId": 1,
#   "id": 101
# }
```

- PATCH

```sh
curl -X PATCH https://jsonplaceholder.typicode.com/posts/1 \
              -H 'Content-Type: application/json' \
              -d '{"title":"foobaloo"}'

# response:
# {
#   "userId": 1,
#   "id": 1,
#   "title": "foobaloo",
#   "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et
#            cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum
#            est autem sunt rem eveniet architecto"
# }
```

- DELETE

```sh
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1 \
               -H 'Content-Type: application/json'

# response:
# {}
```
