# Challenge Neural Works

## Introducción

En este documento se detallará el funcionamiento básico de como fue el desarrollo y despliegue de la solución al challenge propuesto en el archivo *README DE .pdf*. Dentro de este repositorio se encuentra la solución al problema dividida en 2 jupyter notebooks, donde se detalla su funcionalidad en la sección **Desarrollo**. Finalmente, para el despliegue de este repositorio, se aconseja seguir las instrucciones presentes en la sección **Despligue**.

## Despliegue

Para el despliegue de la solución, es necesario el levantamiento de contenedores en docker, cabe destacar que para esto, simplemente fue realizado mediante contenedores con imagenes disponibles en docker hub y scripts presente en los jupyter notebooks para complementar las librerías faltantes al ambiente. Para el despliegue de los contenedores, se deben realizar los siguientes comandos en su terminal:

	1. Clonación del repositorio.
	```
	git clone @
	```
	2. Dirigirse a la ruta del repositorio clonado
	```
	cd ruta\del\repositorio
	```
	3. Despliegue del contenedor de la base de datos Postgres
	```
	docker volume create pgdata
	docker run -d -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=nw_challenge -p 5432:5432 -v pgdata:/var/lib/postgresql/data --name postgres postgres:15-alpine
	```
	4.  Despliegue del contenedor con el ambiente para los jupyter notebooks.
	```
	docker run -d -p 8888:8888 -v $PWD/:/home/jovyan/work --name challenge_neuralworks jupyter/minimal-notebook
	```
	5. Configuración de la red local para que ambos contenedores se puedan comunicar.
	```
	docker network create network_challenge
	docker network connect network_challenge challenge_neuralworks
	docker network connect network_challenge postgres
	```
	6. Para el ingreso al ambiente del jupyter notebook, se debe ingresar a la página *http://localhost:8888/*, donde se requerirá ingresar una token. Para obtener la token, se puede ingresar desde la terminal directamente al ambiente usando el comando
	```
	docker exec -it challenge_neuralworks bash
	```
	Para luego ingresar el comando
	```
	jupyter notebook list
	```
	el cual desplegará un mensaje similar al siguiente
	```
	Currently running servers:
	http://fd62eb5d5355:8888/?token=017919be87e4ded0f51aa7362e3a36e3cd0225e7c2c5da86 :: /home/jovyan
	```
	7. Finalmente, dentro de cada jupyter notebook se encontrará la sección **Script de despliegue**, las cuales deben ser ejecutadas antes de la visualización de la solución al desafio.


## Desarrollo

Para el desarrollo de esta solución, se utilizaron dos archivos *.ipynb*, donde cada uno cumple un rol en específico. El archivo *Challenge.ipynb* contiene la mayoría de las soluciones con respecto al item 1 y 2, por otra parte, el archivo *Challenge_Informe_datos.ipynb* contiene la solución a la segunda parte del item 2 que se relaciona con la muestra de ingesta de datos. Dentro de cada notebook queda especificado la metodología de desarrollo y los algoritmos ocupados para la solución final.