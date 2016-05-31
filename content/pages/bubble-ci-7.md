+++
draft  = false
title  = "bubble-ci-7"
+++

Prebuilt Apache Cloudstack based on Master branch,
Virtualized mcc-bubble comprised of:

* 1 CloudStack management server instance (2vcpu@8GB)
* 2 KVM instance (4vcpu@12GB)
* Public IP: `69.196.164.62`


`~/.ssh/ccc2016mtl_rsa`
```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAzZpSKqw4P8wRoMTq8x1GrKmgF+p8dYUCyrBNd3qbK9IeNAuf
/c4lhVbN4g6f21GezP48b8oybq4JyGwsKR4hyuk/jpV65J0m7h4uH52wKbLKWdry
sZ+wwlOZRM8MreZwdqBbyJUKXMKWFfiXvjSRg/NdqNooEpVHVJ1p0Uvu6uEk9n9f
7q0JyYOfR10oeStiA1+LLFshDZZIFH6mjUvi0IZ0W/UEFQawJ2naUM7ubg6be+8c
485ldpK2tOBDi1qaEdI57A1t+bAWzixiBquOkH2RxXD7921LAufovGmROqBeoVi4
Wl/gouqa0LlBIS4t/Uu3yL9Ep/14bLwomHrzeQIDAQABAoIBAEPpOECuF/pCnoP6
5xwcTG7VrHKZ2jg7Efv/FedkEQL9aUqJmHQN9mi/jkufxv97Szestiu6nsPeKo8P
49pFAKZ0OrEPAMOogOZgA54fyMNNMfdSEZ3IAGt/j32h4i9CkV0thIORbxXKlCZ3
sS97T6FE7mfKfzf8JM53HC+spCLiWWdBE2fayVPd03v+rLYI2oZfDD7GnSCOI2RB
tHx5LjHZWRojTdsIL+3yjfxZhFuZhS2mRZkObzJC5fM4xEvqZlAi7kKwm96NS13h
F7x7weUMHIfIm+AIsclLHYmVO/36HqIRIdz4DrvAAJwwLuQb1Ta/WxYKJX1uFy+I
R4/aU7ECgYEA6ySjWSlv6ReRLqY4FcEYUg1EqOpHCXkI6lNXhOn+NT/05UxUO66o
BomktckOBuBxGHxX9jYqqzezvxBj8wxrOIyCdaHi2Lki0bjT1/DRhcydrf7xjOBC
81z5F63T0pxz7oyIFBiQ8FphbewEMe9yQrTbcA+yhdMs1pt7qzbruw0CgYEA39bq
Vw8OHeJ0uWLIp81zDGAYguT2FyQGB1wrIbTtpFoRQgIVFLjAAPlnq3qxCY1JHH/Z
6Bcv8mNFSNDKItC3XqcJa5kqlpYrxrsniVhnO88Y9K0X+KTxJ0SFtUTnrlSvTGZS
yfUg9M2eR93TsCTxA7h/BHmQfnR18S/a+SbVDx0CgYEA4uQObilKn8qqvy2KLouM
sRe2aZrtgpl0Xc6fQ1QZgz48SsjU+mW0IeLMuM/QphgJaMwKgDuR/nYYDcN9/fa8
uurxsxnK7r3teBn054eqVIW0nEDEyN9YGsVaYVvMaYunXcXiRCnUKOe83TkAb0KR
qQYkO0QaSYET4dxTf0jWOz0CgYBUmsP6Yftg+k5KH/ddzX7Vx6CcIPSPLJOGxqSa
2esUuuJZA7Z6HZadB6fSnc46oQdoWT7Axbrer/zpF9m/LQqSISqjW8JIJrynIehA
toRWi+GP4bj0x0tLH1A2grPbJbEYfHiAU0HApdNUsJiptFzQnjSMOXKPCW/m2MK4
d6ACVQKBgQDiFxP9JZjLQIfAybkblvqiiStsxFUafmZNDYa74UdFmBf5svMC2LxA
MCjQL9V8FBOHR8j9iEeOJtvHUnUwa9nf510CU0aPjiL7P6Qe8+3MTMdEvrX7I8+u
EjKEGR/c5Cq8CS+BnHdBHbM/sO0m/RDAGt+pMNPDHyY8NAYbg0Z3Ew==
-----END RSA PRIVATE KEY-----
```


Access this lab env:
--------------------

Add the private key to your SSH key collection, copy the private key content into
```~/.ssh/ccc2016mtl_rsa```, then change mod: ```chmod 600 ~/.ssh/ccc2016mtl_rsa```

SSH to the bubble system + acs UI port forwarding on localhost:
```
ssh cccna@69.196.164.62 -i ~/.ssh/ccc2016mtl_rsa -L 8080:cs1.cloud.lan:8080
```
http://localhost:8080/client
admin / password

SSH to the management server:
```
[cccna@bubble-ci-7 ~]$ ssh cs1
```

if you need to SSH to KVM servers:
```
[cccna@bubble-ci-7 ~]$ ssh kvm1
[cccna@bubble-ci-7 ~]$ ssh kvm2
```


Run a test case:
----------------

Default run:
```
[cccna@bubble-ci-7 ~]$ ssh cs1
[root@cs1 ~]# cd /data/shared/helper_scripts/cloudstack
[root@cs1 ~]# ./run_marvin_router_tests.sh /data/shared/marvin/mct-zone1-kvm1-kvm2.cfg
```

Example of custom test case:
```
[cccna@bubble-ci-7 ~]$ ssh cs1
[root@cs1 ~]# cd /data/git/cs1/cloudstack/test/integration
[root@cs1 ~]# nosetests --with-marvin \
  --marvin-config=/data/shared/marvin/mct-zone1-kvm1-kvm2.cfg \
  -s -a tags=advanced,required_hardware=true \
  smoke/test_password_server.py
```


Where is git?
-------------

To change CloudStack code and rebuild, the git repo is at
`/data/git/cs1/cloudstack`. 

To test your own branch or fork with bubble:
```
cd /data/git/cs1/cloudstack
git remote add mygithub <...link_to_github...>
git pull
```


Reset the environment
---------------------

```
[cccna@bubble-ci-7 ~]$ cd /data/shared/deploy
[cccna@bubble-ci-7 ~]$ ./kvm_local_deploy.py -x kvm1 && ./kvm_local_deploy.py -x kvm2
[cccna@bubble-ci-7 ~]$ cd /data/shared/helper_scripts/cleaning
[cccna@bubble-ci-7 ~]$ sudo ./clean_storage.sh
[cccna@bubble-ci-7 ~]$ ssh cs1
[root@cs1 ~]# cd /data/shared/helper_scripts/cleaning
[root@cs1 ~]# ./delete_zone.sh -dlocalhost -ucloud -pcloud -z1
```

Then redeploy CloudStack:
```
[cccna@bubble-ci-7 ~]$ ssh cs1
[root@cs1 ~]# cd /data/shared/helper_scripts/cloudstack
[root@cs1 ~]# ./build_run_deploy_test.sh -m /data/shared/marvin/mct-zone1-kvm1-kvm2.cfg
```

Links
-----

* https://github.com/MissionCriticalCloud/bubble-toolkit
