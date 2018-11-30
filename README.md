# Docker Confd Config

This image provides a configuration file `/confd-out/config` from environment variables via volume to be used in other containers.

# Example configuration

Using nginx just to have a running container, so compose up will not exit. Replace nginx with your service and use the configuration file directly or e.g. via symlinks (`ln -s /confd-out/config /etc/nginx/conf.d/custom.conf`).

Write a `docker-compose.yml` (included in github source):

```
version: "3.2"

volumes:
  confd-out:

services:
  nginx:
    image: nginx:alpine
    volumes:
    - confd-out:/confd-out

  confd:
    image: intershopde/docker-confd
    environment:
      CONFIG_CONTENT: |
        multiline
        configuration
        with empty line

    volumes:
    - confd-out:/confd-out
```

Then run:

```
docker-compose up -d
docker exec -it docker-confd_nginx_1 /bin/sh
/ # cat /confd-out/config
> multiline
> configuration
> with empty line
>
/ # exit
docker-compose down -v
```

# License

Copyright 2018 Intershop Communications.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

## Third-party License confd

Github: https://github.com/kelseyhightower/confd

Licensed under the MIT license.

```
Copyright (c) 2013 Kelsey Hightower

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
