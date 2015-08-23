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