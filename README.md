# Sistema de Carga de Datos del S2 y S3 - Oauth 2.0

Este sistema está diseñado para la carga y administración de datos del Sistema de Servidores Públicos que Intervienen en Procedimientos de Contratación (S2) y el Sistema de los Servidores Públicos y Particulares Sancionados (S3)

El sistema de carga de datos  permite a los generadores de datos transferir a las Secretarías Ejecutivas Anticorrupción Estatales los datos de los sistemas 2 y 3, que serán consultados desde la PDN a través de los mecanismos de comunicación desarrollados.

Por su parte, las Secretarías Ejecutivas Anticorrupción Estatales fungirán dentro del sistema como entidades concentradoras de los datos, sin embargo, el control y administración de los mismos será responsabilidad de los generadores/proveedores de datos.

## Tecnologías utilizadas

|Tecnología|Versión|Descripción|
|----------------|-------------------------------|--------------------------------------------------------------|
|Node.js|12.18.2|Entorno base de JavaScript, se usa como motor de ejecución para otras tecnologías del proyecto.|
|Mongoose|5.9.26|Funciona como biblioteca para realizar la conexión e interacción con la base de datos.|
|JWT|8.5.1|Estándar abierto (RFC-7519) basado en JSON para crear un token que sirva para enviar datos entre aplicaciones o servicios y garantizar que sean válidos y seguros.|

## Primeros pasos

### Descargar repositorio
```bash
git clone https://github.com/PDNMX/piloto_sistema_oauth20.git
```

### Variables de entorno
```bash
vim config/config.env
```
```bash
#Expiration token in seconds
EXT=600
#Expiration refresh token in seconds
RTEXT=900
#SEED
SEED=<seed>

#MONGO CONFIG

USERMONGO=admonP2
PASSWORDMONGO=<password>
HOSTMONGO=<IP_HOST>:27017
DATABASE=administracionUsuarios
PORTSERVER=9004
```



### Archivo docker-compose.yml, para agregar junto a los demás servicios

Si está siguiendo la guía puede deberá agregar la siguiente sección a su archivo docker-compose.yml general.

```YAML
  oauth20v2:
	restart: always
	container_name: oauth20v2
	build:
  	  context: piloto_sistema_oauth20
  	  dockerfile: Dockerfile
	ports:
  	- 9004:9004
    links:
      - mongodb
    depends_on:
      - mongodb
```

### Archivo docker-compose.yml, para usar de forma independiente
Si lo que desea es hacer un despliegue independiente puede crear un archivo docker-compose.yml dentro de la carpeta del código.
```YAML
version: '3.1'
services:
  oauth20v2:
	restart: always
	container_name: oauth20v2
	build:
  	  context: piloto_sistema_oauth20
  	  dockerfile: Dockerfile
	ports:
  	- 9004:9004
```


### Construir el contenedor
```bash
docker-compose build oauth20v2
```

### Iniciar el contenedor
```bash
docker-compose up -d oauth20v2
```

### Consultar los logs
```bash
docker-compose logs -f oauth20v2
```
