---
Author: CarefreeLife98
Date: 2023-11-02T19:53:00
Agenda: 
tags:
  - Linux
---
# Docker Mount
## Docker Bind Mount


<br><br>
## Docker Volume Mount
```
$ docker run --name httpd -d -p 8090:80 -v /home/ubuntu/docker:/usr/local/apache2/htdocs httpd
```
> 위 처럼 -v 옵션을 사용하여 Volume 을 마운트 할 수있다.
> - Docker Container 와 Local 의 Directory