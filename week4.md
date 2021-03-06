# Sharing resources

#### 10- Consider the network below. Compute the max-min fair allocation for the hosts in this network assuming that nodes Sx always send traffic towards node Dx. Furthermore, link R1-R2 has a bandwidth of 10 Mpbs while link R2-R3 has a bandwidth of 20 Mbps.

Comment procéder? On se rappelle du critère de min-max fair: 
> Un réseau est dit min-max fair si on ne peut changer l'allocation 
> de bande passante entre un dispositif et un routeur qu'en diminuant
> le débit d'un autre lien (Sx-Dx) de débit inférieur ou égal

Pour l'exercice, cela donne
![Résolution de l'exercice](https://raw.githubusercontent.com/xlambein/lingi1341/master/imgs/S5.png)

(notons que l'algorithme nécessite un peu de feeling pour voir quels liens optimiser)

#### 13- Compute the max-min fair bandwidth allocation in the network below.

![Debut de résolution](https://raw.githubusercontent.com/xlambein/lingi1341/master/imgs/question6.2.png)

# The DNS
#### 1- What are the IP addresses of the resolvers that the dig implementation you are using relies on [1] ?

Les adresses IP des résolvers sont situées dans le fichier /etc/resolv.conf

#### 2- What is the IPv6 address that corresponds to inl.info.ucl.ac.be ? Which type of DNS query does dig send to obtain this information ?

On utilise la commande `dig AAAA inl.info.ucl.ac.be`

Et elle renvoie: 
```
dnssrv.info.ucl.ac.be.	592046	IN	AAAA	2001:6a8:308f:1::d
```

L'addresse IP est donc `2001:6a8:308f:1::d` et correspond à une requête de type A (network address) 

#### 3- Which type of DNS request do you need to send to obtain the nameservers that are responsible for a given domain ?

Il faut envoyer une requête de type `ns` (name server)

#### 4- What are the nameservers that are responsible for the be top-level domain ? Is it possible to use IPv6 to query them ?

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

#### 5- When run without any parameter, dig queries one of the root DNS servers and retrieves the list of the names of all root DNS servers. For technical reasons, there are only 13 different root DNS servers. This information is also available as a text file from http://www.internic.net/zones/named.root . What are the IPv6 addresses of all these servers.

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

# Internet email protocols

#### Use your preferred email tool to send an email message to yourself containing a single line of text. Most email tools have the ability to show the source of the message, use this function to look at the message that you sent and the message that you received. Can you find an explanation for all the lines that have been added to your single line email ?

```
MIME-Version: 1.0
Received: by 10.76.167.2 with HTTP; Tue, 13 Oct 2015 06:07:50 -0700 (PDT)
Date: Tue, 13 Oct 2015 15:07:50 +0200
Delivered-To: alexisclarembeau@gmail.com
Message-ID: <CAKyB3NQuMCuN3Ym453CEk7-1uC8_XM0gqX-_b=e5V-mPYuHRLg@mail.gmail.com>
Subject: Voici le titre du message
From: Alexis Clarembeau <alexisclarembeau@gmail.com>
To: alexis.clarembeau@gmail.com
Content-Type: multipart/alternative; boundary=047d7b3442e416298e0521fc22a5

--047d7b3442e416298e0521fc22a5
Content-Type: text/plain; charset=UTF-8

Et voici le contenu du message

--047d7b3442e416298e0521fc22a5
Content-Type: text/html; charset=UTF-8

<div dir="ltr">Et voici le contenu du message</div>

--047d7b3442e416298e0521fc22a5--
```

 * MIME: version du protocole d'envoi de fichiers 
 * Received: date de réception
 * Date: date d'envoi
 * Delivered-To: destinataire effectif
 * Message-ID: identifiant unique du message
 * Subject: titre
 * From: auteur
 * To: destinataire souhaité
 * Content-Type: type de contenu (fichier)

La suite est composé de portions de texte au format HTML et de sommes de contrôle. 

#### The TCP protocol supports 65536 different ports numbers. Many of these port numbers have been reserved for some applications. The official repository of the reserved port numbers is maintained by the Internet Assigned Numbers Authority (IANA) on http://www.iana.org/assignments/port-numbers [3] Using this information, what is the default port number for the POP3 protocol ? Does it run on top of UDP or TCP ?

Par défaut, il s'agit du port 110 et fonctionne via TCP

#### The Post Office Protocol (POP) is a rather simple protocol described in RFC 1939. POP operates in three phases. The first phase is the authorization phase where the client provides a username and a password. The second phase is the transaction phase where the client can retrieve emails. The last phase is the update phase where the client finalises the transaction. What are the main POP commands and their parameters ? When a POP server returns an answer, how can you easily determine whether the answer is positive or negative ?

 * `USER nom_de_votre_compte`: permet de spécifier son nom d'utilisateur
 * `PASS mot_de_passe`: permet de spécifier son mot de passe
 * `DELE numéro_du_message`:supprime le message spécifié
 * `LIST`: liste les messages et leurs tailles 
 * `RETR numéro_du_message`: récupère le contenu d'un message
 * `STAT`: indique le nombre total de messages et la taille de leur ensemble
 * `TOP numéro_du_message nombre_de_lignes`: affiche le début d'un message

 Si la première ligne de la réponse commence par OK, elle est positive. Sinon, elle est négative. 
 
 #### On smartphones, users often want to avoid downloading large emails over a slow wireless connection. How could a POP client only download emails that are smaller than 5 KBytes ?
 
On utilise d'abord LIST pour récupérer uniquement les petits messages. Ensuite, on fait retr uniquement sur les petits messages. 

