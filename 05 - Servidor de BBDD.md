# 05 - Servidor de BBDD

## - Introducció

El servidor de BBDD és el servidor encarregat de instal·lar el programa de MYSQL per a la base de dades i mantenr aquesta. També ha de poder permetre al servidor Web poder acceir a la base de dades per a que el servidor web pugui mostrar als clients la base de dades que només volem que es mostri.

## Desplegament del servidor de BBDD

Primer instal·lem el servei MYSQL, i després per verificar que s'ha instal·lat amb èxit executem la comanda "sudo systemctl status mysql" i aixì podem veure l'estat de MYSQL i que s'ha instal·lat.

![Instal·lació de MYSQL](./Imatges/Sprint1/inssql.png)
Fig. Instal·lació de MYSQL al servidor de BBDD.

![Systemctl status MYSQL](./Imatges/Sprint1/statusmysql.png)
Fig Observació de estat de MYSQL.

Ara creem un usuari el qual només es pot utilitzar desde la IP del servidor web, així cap altre sistema pot accedir a la base de dades, si no es des de el servidor WEB. També li donem permisos de SELECT, ja que només ens interessa que pugui veure la base de dades i no editar-la, aixì també evitem el fet de que si es fiquen al servidor web puguin editar o borrar la base de dades.

![Creació de usuari mysql](./Imatges/Sprint1/creausuari.png)
Fig. Creació d'usuari exclusiu per al servidor Web.

![Permisos usuari myslq](./Imatges/Sprint/Permisos_bchecker.png)
Fig. Assignació de permisos a l'usuari *bchecker*.

---

[← 04 - Servidor Web + FTP](<04 - Servidor Web + FTP.md>) -------------------------------------------------------------------------------------------- [06 - Client Windows i Linux →](<06 - Client Windows i Linux.md>)
