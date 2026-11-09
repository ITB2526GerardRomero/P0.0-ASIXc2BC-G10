# 01 - Arquitectura de la xarxa

## Antiga arquitectura de la xarxa

![Imatge antiga Xarxa](./Imatges/Sprint1/image_1.jpg)
Fig. 1.1. Antiga arquitectura de la nostra xarxa.

La nostra xarxa es separa en tres parts, la part d’accés a internet, la xarxa dmz i la xarxa intranet.

La zona principal, on es troba el servidor router és la zona d’accés a internet, on el router redirecciona el tràfic necessari per a als servidors i els clients. Redirecciona tot el tràfic forense d’internet cap a la xarxa DMZ, i permet que a xarxa intranet pugui accedir a la DMZ, però que la xarxa DMZ no pugui accedir a la xarxa intranet.

La **xarxa DMZ** es defineix per obtenir tots els servidors de serveis que es mostren de manera pública a la gent fora de l’empresa, la xarxa DMZ conté els següents serveis:
- **Servidor Web**: És el servidor que permet als usuaris clients fora de l’empresa poder accedir a la pàgina web de l’empresa.
- **Servidor FTP**: Permet als usuaris descarregar arxius dels nostres serveis i en el cas de que ho necessitin també pujar els seus a la pàgina web.
- **Servidor de bases de dades**: Es troba a la xarxa DMZ però només el servidor web té accés a aquest de lectura, tots els altres ports que no siguin necessaris per connectar el servidor web i aquest estan bloquejats mitjançant un firewall això es fa només per a que el servidor web pugui mostrar la informació de la base de dades i no pugui accedir cap persona.

La **xarxa intranet** és la xarxa que conté els serveis privats, és a dir, que no poden ser accedits per persones fora de l’empresa per aquest cas només hi ha:
- **Servidor DHCP + DNS**: És el indicat per a crear el pool de les ip per posar automàticament als serveis i és el que resol els noms de R-N10 i R pels altres sistemes.
- **Client Windows i Client Ubuntu**: Són els que s’utilitzaran per a posar a prova tots els serveis i verificar de que tot funciona.

## Nova arquitectura de la xarxa

Després de veure com vam estructurar la xarxa, vam veure que tenia certes parts errades i fent que la estructura de la xarxa es podia millorar.

![Imatge Xarxa Actualitzada](./Imatges/Sprint1/nova%20arquitectura.jpg)
Fig. 1.2. Nova arquitectura de la nostra xarxa

De la mateixa manera que l'anterior estructura de xarxa la zona principal, on es troba el servidor router és la zona d’accés a internet, on el router redirecciona el tràfic necessari per als servidors i els clients. Redirecciona tot el tràfic forense d’internet cap a la xarxa DMZ, i permet que la xarxa intranet pugui accedir a la DMZ, però que la xarxa DMZ no pugui accedir a la xarxa intranet.
Però ara, ja que el router pot accedir a ambdós xarxes té el DNS ja que surt com a representant. També té el DCHP, ja que així pot posar IPs de manera automàtica a tots els ordinadors de les dues xaxares per aixì poder diferenciarlos.

La **xarxa DMZ** segueix amb els mateixos servidors que abans, però hem ajuntat el **Servidor FTP** amb el **Servidor Web** ja que no hi havia necessitat de crear un tercer servidor per a FTP.

De la mateixa manera a la **xarxa Intranet** hem deixat només els clients. D'aquesta manera hem pogut reduir el numero de ordinadors encesos i aixì tenir una xarxa més compacta.

---

[← README](README.md) --------------------------------------------------------------------------------------------------------------------------------------- [02 - Direccionament de IPs →](<02 - Direccionament de IPs.md>)
