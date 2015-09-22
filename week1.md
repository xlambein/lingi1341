# Solutions semaine 1

## Open questions

1. L'émetteur commence au temps zéro à transmettre des bits au récepteur. Il y a `8*d` bits. À `b` bits par seconde, cela prendra `8*d / b` secondes. Une fois cela fait, il faut simplement attendre `t` secondes pour que le dernier bit passe à travers le fil, ce qui donne un total de `8*d/b + t`.

2. Notons d'abord que la temps pour parcourir le fil est de `1000km * 5µs/km = 5ms`. On peut ensuite séparer le temps d'échange en cinq parties :
  - l'envoi de la trame : `1000b / 1Mbps = 1ms` ;
  - la latence de l'émetteur au récepteur : `5ms` ;
  - les calculs du récepteur : `1ms` ;
  - l'envoi de l'acknowledgement : `40b / 1Mbps = 40µs` ;
  - la latence du récepteur à l'émetteur : `5ms`.
  
  Cela donne un total de `12.04ms`.

3. L'énoncé est peu clair : pour préciser, c'est la deuxième machine (le receveur / client) qui est concernée par les limitations de vitesses. On a donc un débit de `50Mbps` pour l'envoi de la trame et un débit de `1Mbps` pour l'envoi de l'acknowlegment.
  
  On peut de nouveau séparer le temps pris par une trame en quatre parties :
  - l'envoi de la trame : `125*8b / 50Mbps = 20µs` ;
  - la latence du serveur au client : `10ms` ;
  - l'envoi de l'acknowledgement : `25*8b / 1Mbps = 200µs` ;
  - la latence du client au serveur : `10ms`.
  
  Cela donne un total de `20.22ms` par frame, donc 49.46 frames par seconde, et un débit de `49.46/s * 125*8b ~= 50kbps`.
  
  Pour le deuxième cas, seul l'envoi de la trame change : il passe à `1500*8b / 50Mbps = 240µs`, et le total devient `20.44ms` par frame, donc 48.92 frames par seconde, à peine différent, mais un débit beaucoup plus important de `48.92/s * 1500*8b ~= 600kpbs`. On remarque qu'augmenter la taille de la trame permet d'augmenter nettement le débit (et si on va plus loin, cela permet de s'approcher du débit pur).

4. Pour être raisonnablement sûr que le paquet ou l'acknowledgement a été perdu, il faut estimer le temps d'aller-retour du paquet (RTT, Round Trip Time), puis y ajouter un marge d'erreur, qui dépendra en partie de la fiabilité du réseau sur lequel le paquet transite. Il existe de nombreux algorithmes pour y arriver.
  
5. C'est une mauvaise idée : en effet, comme le CRC ne permet plus de vérifier si le numéro de séquence ou la taille a été corrompu, il est possible que le receveur pense avoir reçu la bonne information alors que ce n'est pas le cas. Par exemple, si la taille a été corrompue, le receveur ne lira pas la trame correctement. Si le numéro de séquence a été corrompu, le receveur sera décalé au niveau des données. Dans les deux cas, il n'aura aucun moyen de le savoir, et continuera en pensant donc avoir les données correctes.

6. Calculons d'abord le temps total pour l'envoi et le feedback d'une trame. Ce calcul a déjà été fait plusieurs fois aux questions 2 et 3, nous irons donc plus vite. Il y a au total :
  - `D + 2*c` bytes transmis à `B` bits par seconde, ce qui prend `8*(D + 2*c) / B` secondes;
  - deux délais de `s` secondes, donc `2*s` secondes.
  
  Cela donne un total de `(D + 2*c)/B + 2*s` secondes pour une trame. Chaque trame comprenant `D` bits utiles, cela nous donne un débit utile de `8*D / [8*(D+2*c)/B + 2*s]` (en bits par seconde).
  
  Remarquons que cette expression s'approche de `B` quand `D` augmente. On voit donc à nouveau qu'il est bon d'avoir de longues trames *s'il n'y a pas d'erreur de transmission*. En pratique, il faudra se limiter pour éviter de devoir envoyer une trame de nombreuses fois avant de la recevoir correctement.

7. À faire (pour qui veut s'amuser à faire des dessins).

8. Voir 7.


## Practice

1. Tout d'abord, décrivons comment marche (de manière simplifiée) l'algorithme de calcul de la checksum. Soit une suite de bytes (un message) dont l'on souhaite calculer la checksum. On découpe cette suite en groupes de 16 bits, et on additionne ensemble ces groupes sur 32 bits, de manière à obtenir une somme. Ensuite, on « replie » cette somme sur 16 bits, en découpant les 32 bits en deux groupes de 16 bits et en additionnant ceux-ci.

  Par exemple, ci-dessous nous avons 6 bytes qui se décomposent en trois groupes de 16 bits, dont la valeur décimale est renseignée : `|--5893--|---2----|-60000--|`. La somme obtenue sur 32 bits en additionnant les trois groupes est `65895`, ce qui donne `00000000 00000001 00000001 01100111` en binaire. On la replie ensuite sur 16 bits en faisant `00000000 00000001 + 00000001 01100111`, ce qui donne `00000001 01101000` ou `360` en décimal, qui est la checksum que l'on voulait calculer.
  
  Si l'un des bits change, par exemple dans le premier groupe, la valeur du groupe en question changera (par exemple `5893` pourrait devenir `1797` en mettant le 13e bit à 0). En sommant tous les groupes, on obtiendra une somme différente, qui sera repliée en une checksum différente. Le receveur ayant calculée celle-ci, la comparera à celle qui a été communiquée avec le message, et voyant qu'elles sont différentes, signalera que le message reçu est corrompu.
  
  Un problème pourrait arriver s'il advenait que le même bit était inversé sur deux blocs de 16 bits différents. Par exemple, considérons les groupes `|--5893--|` et `|---2----|`. En binaire, on obtient `|00010111 00000101|` et `|00000000 00000010|`. Si le 3e bit en partant de la droite était inversé dans un des deux groupes, on obtiendrait une checksum différente, et l'erreur serait détectée (voir plus haut). Si par contre on inversait le 3e bit dans les deux, c'est-à-dire `|00010111 00000001|` et `|00000000 00000110|`, `5889` et `6` en décimal, la somme calculée aurait la même valeur car `5893 + 2 = 5889 + 6`, et donc la checksum serait la même. Par conséquent, l'erreur ne sera pas détectée. Le receveur considérerait donc le message corrompu comme étant correct.

2.

3.


## Discussion questions

1.

2.

3.

4.

5.
