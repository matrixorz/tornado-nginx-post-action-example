tornado-nginx-post-action-example
=================================

Sample server and nginx.conf demonstrating a useful technique for how to use nginx post actions after Tornado is done serving a page.

Running
-------

```python ./example.py --logging=debug --debug```

```curl http://127.0.0.1:8080/```

Result
------

Here we see the first request with the 1 second delay.  Nginx logs a lot of the request time which we then pass along to the post action which returns a 202 HTTP code.  Very handy code for logging purposes.

Instead of using pprint we could use coroutines within the prepare statement to asyncronously flush this data off to mongodb (via motor).

Why does it stop after prepare?  Because we called self.finish() which is checked before an HTTP method handler is executed.

```json
[I 140228 08:59:59 web:1728] 200 GET / (127.0.0.1) 1001.66ms
[I 140228 08:59:59 web:1728] 202 GET / (127.0.0.1) 0.36ms
{'duration': 0.000338,
 'end': datetime.datetime(2014, 2, 28, 17, 59, 59, 481580, tzinfo=<UTC>),
 'kwargs': {},
 'request': {'args': [],
             'body': '',
             'full_url': 'http://127.0.0.1/',
             'headers': {'Accept': '*/*',
                         'Connection': 'close',
                         'Host': '127.0.0.1',
                         'User-Agent': 'curl/7.26.0',
                         'X-Forwarded-For': '127.0.0.1',
                         'X-Post-Action': '1',
                         'X-Real-Ip': '127.0.0.1'},
             'kwargs': {},
             'protocol': 'http',
             'uri': '/'},
 'response': {'headers': {'Upstream-Addr': '127.0.0.1:8081',
                          'Upstream-Content-Type': 'application/json; charset=UTF-8',
                          'Upstream-Request-Time': '1.002',
                          'Upstream-Server': 'TornadoServer/3.2',
                          'Upstream-Status': '200',
                          'Upstream-Time': '1.002'}},
 'start': datetime.datetime(2014, 2, 28, 17, 59, 59, 481251, tzinfo=<UTC>)}

```

See
---

http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html
