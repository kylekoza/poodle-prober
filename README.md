# POODLE Prober

Probe your poodle. Just to be safe.

Scan a netblock for SSLv3 enabled servers.

# Setup

```
# apt-get install python3-ipy
```

or, if that's not available

```
# apt-get install python3-pip
# pip3 install IPy
```

# Usage

```
sslv3check.py [-p port port ...] [-n <network/mask> <network/mask> ... OR -H <hostname> <hostname> ...] [-t] [-P]
    -p port to connect to (default=443)
    -t check if SSLv3 is enabled and TLSv1 is not enabled
       otherwise just see if SSLv3 is enabled
    -P run checks on networks in parallel
```

Just look for anyone with SSLv3 turned on:

```
$ python3 sslv3check.py -n 10.0.1.0/24
10.0.1.1:443 SSLv3 [SSL: SSLV3_ALERT_HANDSHAKE_FAILURE] sslv3 alert handshake failure (_ssl.c:598)
10.0.1.2:443 SSLv3 timed out
10.0.1.3:443 SSLv3 timed out
10.0.1.4:443 SSLv3 enabled
10.0.1.5:443 SSLv3 enabled
```

Look for things with SSLv3 turned on and TLSv1 turned off:

```
$ python3 sslv3check.py -n 10.0.1.0/24 -t
10.0.1.1:443 SSLv3 [SSL: SSLV3_ALERT_HANDSHAKE_FAILURE] sslv3 alert handshake failure (_ssl.c:598) TLSv1 enabled
10.0.1.2:443 SSLv3 timed out TLSv1 timed out
10.0.1.3:443 SSLv3 timed out TLSv1 timed out
10.0.1.4:443 SSLv3 enabled TLSv1 not enabled
10.0.1.5:443 SSLv3 enabled TLSv1 enabled
```

Just check one host:

```
$ python3 sslv3check.py -p 443 444 -n 10.0.1.1
10.0.1.1:443 SSLv3 [SSL: SSLV3_ALERT_HANDSHAKE_FAILURE] sslv3 alert handshake failure (_ssl.c:598)
10.0.1.1:444 SSLv3 enabled
```

Check a host by name:

```
$ python3 sslv3check.py -H www.example.com
www.example.com:443 SSLv3 [SSL: SSLV3_ALERT_HANDSHAKE_FAILURE] sslv3 alert handshake failure (_ssl.c:598)
```

Scan multiple networks in parallel:

```
$ python3 sslv3check.py -n 10.0.1.0/24 10.1.0.0/24 -P
10.1.0.1:443 SSLv3 timed out
10.0.1.1:443 SSLv3 timed out
10.1.0.2:443 SSLv3 timed out
10.0.1.2:443 SSLv3 enabled
10.1.0.3:443 SSLv3 timed out
10.0.1.3:443 SSLv3 timed out
```

# Props

- To Kohster for the name and the "TLSv1 disabled" feature suggestion!
- To Kim C for the suggestion that multiple ports be a command line option.
- To Ross V for a patch implementing -H <hostname>
