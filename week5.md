## 1- What are the IP addresses of the resolvers that the dig implementation you are using relies on [1] ?

Les adresses IP des résolvers sont situées dans le fichier /etc/resolv.conf

## 2- What is the IPv6 address that corresponds to inl.info.ucl.ac.be ? Which type of DNS query does dig send to obtain this information ?

On utilise la commande `dig AAAA inl.info.ucl.ac.be`

Et elle renvoie: 
```
dnssrv.info.ucl.ac.be.	592046	IN	AAAA	2001:6a8:308f:1::d
```

L'addresse IP est donc `2001:6a8:308f:1::d` et correspond à une requête de type A (network address) 

## 3- Which type of DNS request do you need to send to obtain the nameservers that are responsible for a given domain ?

Il faut envoyer une requête de type `ns` (name server)

## 4- What are the nameservers that are responsible for the be top-level domain ? Is it possible to use IPv6 to query them ?

On utilise cette commande `dig www.google.be @a.root-servers.net -t ns`
Elle retourne:
```
a.ns.dns.be.		172800	IN	A	194.0.6.1
b.ns.dns.be.		172800	IN	A	194.0.37.1
c.ns.dns.be.		172800	IN	A	194.0.43.1
d.ns.dns.be.		172800	IN	A	194.0.44.1
x.ns.dns.be.		172800	IN	A	194.0.1.10
y.ns.dns.be.		172800	IN	A	120.29.253.8
a.ns.dns.be.		172800	IN	AAAA	2001:678:9::1
b.ns.dns.be.		172800	IN	AAAA	2001:678:64::1
c.ns.dns.be.		172800	IN	AAAA	2001:678:68::1
d.ns.dns.be.		172800	IN	AAAA	2001:678:6c::1
x.ns.dns.be.		172800	IN	AAAA	2001:678:4::a
y.ns.dns.be.		172800	IN	AAAA	2001:dcd:7::8
```

On peut envoyer des queries en IPV6 via `dig AAAA a.ns.dns.be`
Et qui retourne: 
```
2001:678:9::1
```

## 5- When run without any parameter, dig queries one of the root DNS servers and retrieves the list of the names of all root DNS servers. For technical reasons, there are only 13 different root DNS servers. This information is also available as a text file from http://www.internic.net/zones/named.root . What are the IPv6 addresses of all these servers.

 * A.ROOT-SERVERS.NET.      3600000      AAAA  2001:503:ba3e::2:30
 * B.ROOT-SERVERS.NET.      3600000      AAAA  2001:500:84::b
 * C.ROOT-SERVERS.NET.      3600000      AAAA  2001:500:2::c
 * D.ROOT-SERVERS.NET.      3600000      AAAA  2001:500:2d::d
 * F.ROOT-SERVERS.NET.      3600000      AAAA  2001:500:2f::f
 * H.ROOT-SERVERS.NET.      3600000      AAAA  2001:500:1::803f:235
 * I.ROOT-SERVERS.NET.      3600000      AAAA  2001:7fe::53
 * J.ROOT-SERVERS.NET.      3600000      AAAA  2001:503:c27::2:30
 * K.ROOT-SERVERS.NET.      3600000      AAAA  2001:7fd::1
 * L.ROOT-SERVERS.NET.      3600000      AAAA  2001:500:3::42
 * M.ROOT-SERVERS.NET.      3600000      AAAA  2001:dc3::35


