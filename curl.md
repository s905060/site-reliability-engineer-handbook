# CURL

To store the output in a file, you an redirect it as shown below. This will also display some additional download statistics.

```
$ curl http://www.centos.org > centos-org.html
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 27329    0 27329    0     0   104k      0 --:--:-- --:--:-- --:--:--  167k
```

Save the cURL Output to a file

```
$ curl -o mygettext.html http://www.gnu.org/software/gettext/manual/gettext.html
```