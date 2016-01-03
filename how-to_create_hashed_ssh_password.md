# How-To create hashed ssh passsword

```
openssl passwd -1 -salt <8 random character for your choice> <the password you want>
```

```
openssl passwd -1 -salt 'random-phrase-here' 'your-password-here
```