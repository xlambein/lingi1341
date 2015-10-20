
# The HyperText Transfer Protocol

#### System administrators who are responsible for web servers often want to monitor these servers and check that they are running correctly. As a HTTP server uses TCP on port 80, the simplest solution is to open a TCP connection on port 80 and check that the TCP connection is accepted by the remote host. However, as HTTP is an ASCII-based protocol, it is also very easy to write a small script that downloads a web page on the server and compares its content with the expected one. Use telnet to verify that a web server is running on host cnp3book.info.ucl.ac.be [4]

`telnet cnp3book.info.ucl.ac.be 80` indique qu'il est possible de se connecter sur le serveur

#### Instead of using telnet on port 80, it is also possible to use a command-line tool such as curl Use curl with the –trace-ascii tracefile option to store in tracefile all the information exchanged by curl when accessing the server.

 * what is the version of HTTP used by curl ? HTTP 1.1
 * can you explain the different headers placed by curl in the request ?

```
GET / HTTP/1.1                      récupère la racine du site via HTTP 1.1
User-Agent: curl/7.35.0             spécifie le logiciel qui envoie le message
Host: cnp3book.info.ucl.ac.be       nom d'hôte du serveur 
Accept: */*                         accepte tous les types de fichiers
```

 * can you explain the different headers found in the response ?

```
HTTP/1.1 200 OK                               réponse favorable 
Date: Tue, 13 Oct 2015 13:20:00 GMT           date d'envoi
Server: Apache/2.2.22 (Debian)                serveur HTTP du serveur
Last-Modified: Wed, 16 Sep 2015 14:05:49 GMT    dernière modification côté serveur
ETag: "32e3dc-15d2-51fddcbae8940"             numéro du contenu demandé
Accept-Ranges: bytes                          indique quels types de 'range request' on accepte
Content-Length: 5586                          taille du contenu 
Vary: Accept-Encoding                         encodage des caractères
Content-Type: text/html                       contenu: page HTML
```
