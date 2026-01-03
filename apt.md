# apt

## Debianが期限切れになったので、BusterからBullseyeに更新する

/etc/apt/source.list
```
#deb http://cdn-aws.deb.debian.org/debian buster main
#deb-src http://cdn-aws.deb.debian.org/debian buster main
#deb http://security.debian.org/debian-security buster/updates main
#deb-src http://security.debian.org/debian-security buster/updates main
#deb http://cdn-aws.deb.debian.org/debian buster-updates main
#deb-src http://cdn-aws.deb.debian.org/debian buster-updates main
#deb http://cdn-aws.deb.debian.org/debian buster-backports main
#deb-src http://cdn-aws.deb.debian.org/debian buster-backports main

deb http://cdn-aws.deb.debian.org/debian bullseye main
deb-src http://cdn-aws.deb.debian.org/debian bullseye main
#deb http://security.debian.org/debian-security bullseye/updates main
#deb-src http://security.debian.org/debian-security bullseye/updates main
deb http://cdn-aws.deb.debian.org/debian bullseye-updates main
deb-src http://cdn-aws.deb.debian.org/debian bullseye-updates main
deb http://cdn-aws.deb.debian.org/debian bullseye-backports main
deb-src http://cdn-aws.deb.debian.org/debian bullseye-backports main
```
