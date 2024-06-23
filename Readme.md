# Proyecto: Creación Entorno Herramientas del WAAS (Weapons As A Service)

En este repositorio se ofrecen los recursos para trabajar en local para poder desarrollar nuevas funcionalidades en el Informacional del WAAS. Los servicios utilizados para este proyecto serán los que veremos a continuación.

### Apache Airflow (2.8.4)
Apache Airflow es una plataforma de gestión de flujo de trabajo de código abierto escrita en Python, donde los flujos de trabajo se crean a través de scripts de Python.
### MongoDB (6.0.4)
MongoDB es un sistema de base de datos NoSQL, orientado a documentos y de código abierto. 
### Mongo-Express
Ofrecerá una interfaz Web de MongoDV que pernitirá revisar de una forma visual los datos en MongoDB.

> [!IMPORTANT]
> Las versiones se han definido previamente por el equipo de Plataforma, si se cambia la versión en alguna momento, se deben cambiar los ficheros correspondiente y avisar a los compañeros**

## Prerequisitos
La instalación de los servicios comentados anteriormente se realizará en docker por lo tanto será necesario lo siguiente.
1. Tener instalado Docker Engine y el plugin de docker compose

## Instalación en local
Los recursos que se ofrecen para la instalación de los servicios son los siguientes
- requirements.txt: Librerías que necesitaremos utilizar en apache airflow.
- Dockerfile: Siguiendo las intrucciones oficiales de Airflow, se recomieda generar una imagen custom utilizando un requirements.txt con las librerías que se necesitarán en nuestro proyecto.
- docker-compose: fichero que administra la administración del ciclo de vida de los servicios levantados, permitiendo iniciar, detener, ver estado... 

Estos serían los únicos recursos necesarios inicialmente, otros como el airflow.cfg podría ser necesario si se quisiese a futuro cambiar diferentes parámetros de la configuración inicial, como no es el caso por el momento se hace directamente desde el docker-compose.yml

## Procedimiento de Instalación
En este apartado se definirá el procedimiento a seguir para desplegar el entorno.
1. Buildear la imagen de apache-airflow con las librerías de python necesarias para el proyecto

     ```console 
     docker build -t apache/airflow_2.8.4_custom:v1 .
     ```
    
2. Para separar el despligue del entorno del desarrollo del código, se deberá definir en una variable de entorno establecida por la documentación de airflow con la ruta del proyecto de airflow, es decir, donde estarán dags y resto de código. Ejemplo: 
    ```console 
    export AIRFLOW_PROJ_DIR=/home/jorge-zapata/Documentos/WAAS/fusion_weapons_airflow_dags
    ```
En esta nota debería estar la rama de desarrollo de cada usuario del repositorio fusion_weapons_airflow_dags a partir de la cual se realizará posteriormente el merge a develop para probar en el entorno de desarollo.
3. Desplegar los servicios con docker compose en segundo plano.

    ```console 
    docker compose up -d
    ```

## Guía de uso
Una vez desplegados los servicios se podrá acceder a la interfaz Web de estos, los usuarios están definidos en el docker compose, por lo que podréis acceder utilizando estos, las rutas serán las siguientes:

- http://localhost:8080/home (airflow)
- http://localhost:8082/ (mongo-express)

Al estar los servicios en la misma "network" se podrá atacar desde airflow a mongo sin problema, solo hay que tener en cuenta que si se accede desde airflow, el cliente de mongodb se deberá crear atacando a 
```mongodb://fusiondata:data2024@mongo:27017/``` mientras que si es desde local se accederá atacando a ```mongodb://fusiondata:data2024@localhost:27017/```.

Además también se podría probar la parte de Bigquery utilizando la BigQuery Sandobox (permite probar las funcionales de BigQuery de forma gratuita) https://cloud.google.com/bigquery/docs/sandbox?hl=es-419 siempre y cuando no se publiquen datos reales, o bien utilizar un entorno "staging" si lo tenemos disponible.
