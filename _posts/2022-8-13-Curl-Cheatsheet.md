---
title: "Curl Cheatsheet"
author: zacgra
layout: post
categories: Code
tags: [endpoints, cli]
---

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
