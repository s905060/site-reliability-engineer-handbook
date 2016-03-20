# CURL

To store the output in a file, you an redirect it as shown below. This will also display some additional download statistics.

```
$ curl http://www.centos.org > centos-org.html
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 27329    0 27329    0     0   104k      0 --:--:-- --:--:-- --:--:--  167k
```

Save the cURL Output to a file:

```
$ curl -o mygettext.html http://www.gnu.org/software/gettext/manual/gettext.html
```

Continue/Resume a Previous Download:
```
curl -C - -O http://www.gnu.org/software/gettext/manual/gettext.html
###############            21.1%
```

Pass HTTP Authentication in cURL:
```
$ curl -u username:password URL
```

GET Method:
```
curl -X GET 'http://localhost:5000/locations?id=3'
```

POST method:
```
 curl -X POST --user "user:pass" -d "image_id=ami-84db39ed&hwp_id=m1.small&keyname=marios_key"  http://localhost:3001/api/instances?format=xml 
```

This example assumes you have set your services endpoint at /service and that you have enabled the comment and user resource. So lets first login to a drupal site:
```
curl -X POST -i -H "Content-type: application/json" -c cookies.txt -X POST http://localhost:8888/service/user/login -d '
    {
        "username":"user",
        "password":"password"
    }
    '
```

* `-i` means show http response headers
* `-H` allows you to set http request headers. And drupal services need just the content-type header
* `-c` is to save the cookies on the cookies.txt file. And since we are doing a login this is important
* `-d` allows you to set the request body, which you will be using on drupal services to send the parameters

```
curl -i -H "Content-type: application/json" -b cookies.txt -X POST http://localhost:8888/service/comment -d '
    {
        "nid":"579",
        "subject":"Test Subject",
        "comment_body": { "und": [ { "value": "Test Comment" } ] }
    }
    '
```

* Here we use `-b` instead of `-c` because we now want to _send_ the cookie to let drupal now that we already logged in.

```
curl -i -H "Content-Type: application/json" -b cookies.txt -X POST http://localhost:8888/service/user/logout
```