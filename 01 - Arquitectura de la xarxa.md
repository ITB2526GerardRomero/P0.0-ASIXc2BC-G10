# 01 - Arquitectura de la xarxa



![Imatge Xarxa](./Imatges/Sprint 1/image_1.jpg)



Fig. 1.1. Arquitectura de la nostra xarxa.

La nostra xarxa es separa en tres parts, la part d’accés a internet, la xarxa dmz i la xarxa intranet.

La zona principal, on es troba el servidor router és la zona d’accés a internet, on el router redirecciona el tràfic necessari per a als servidors i els clients. Redirecciona tot el tràfic forense d’internet cap a la xarxa DMZ, i permet que a xarxa intranet pugui accedir a la DMZ, però que la xarxa DMZ no pugui accedir a la xarxa intranet.

La xarxa DMZ es defineix per obtenir tots els servidors de serveis que es mostren de manera pública a la gent fora de l’empresa, la xarxa DMZ conté els següents serveis:

* Servidor web: És el servidor que permet als usuaris clients fora de l’empresa poder accedir a la pàgina web 
* Servidor FTP:
* Servidor de bases de dades:
