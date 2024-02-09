# libersys
Libera de componentes privativos a Debian, Devuan y derivadas. 

## ¿Cómo se instala?

Basta con instalar el paquete .deb desde este repositorio (Releases) o desde el repositorio oficial de Quirinux:

https://repo.quirinux.org/pool/main/c/crealib-libersys/

Una vez descargado se puede instalar con:

sudo apt install ./crealib-libersys_1.0.0_all.deb	

O bien:

sudo dpkg -i ./crealib-libersys_1.0.0_all.deb	

## ¿Cómo se usa?

Una vez instalado, nuestro sistema habrá incorporado dos nuevos comandos: libre y notanlibre.

### libre

Utiliza el programa vrms para detectar los componentes privativos instalados en nuestro sistema. 

### notanlibre

Sirve para volver a instalar aquellos programas que hayan sido desinstalados mediante el comando "libre", sin necesidad de conectarnos a internet.

#### Nota:
Libersys libera los paquetes instalados, aunque mantiene el kernel que se encuentre en el sistema. Para una limpieza completa, recomendamos instalar un núcleo libre GNU:
http://linux-libre.fsfla.org/pub/linux-libre/releases/6.4-gnu/

#### Agradecimientos a:

Riky Linux, por las sugerencias valiosas que me hizo en su canal "Gnuxero":
https://fediverse.tv/w/dfkDrxL2DU5rSTGhiRCJDP


