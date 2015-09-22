# Solutions semaine 1

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
