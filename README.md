## ETL with Python, Docker, PostgreSQL and Airflow

![image](https://drive.google.com/uc?export=view&id=1NUq9DSFlOy5XOurspumIWlpN_pz65V7u)

There are a lot of different tools and frameworks that are used to build ETL pipelines. In this repo I will build an ETL using Python, [Docker](https://www.docker.com/), [PostgreSQL](https://www.postgresql.org/) and [Airflow](https://airflow.apache.org/) tools.
  

### Setup the environment:

1. Create .env file with the environment variables described below:

```

# Meta-Database

POSTGRES_USER=airflow

POSTGRES_PASSWORD=airflow

POSTGRES_DB=airflow


# Airflow Core

AIRFLOW__CORE__FERNET_KEY=UKMzEm3yIuFYEq1y3-2FxPNWSVwRASpahmQ9kQfEr8E=

AIRFLOW__CORE__EXECUTOR=LocalExecutor

AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION=True

AIRFLOW__CORE__LOAD_EXAMPLES=False

AIRFLOW_UID=0

  
# Backend DB

AIRFLOW__DATABASE__SQL_ALCHEMY_CONN=postgresql+psycopg2://airflow:airflow@postgres/airflow

AIRFLOW__DATABASE__LOAD_DEFAULT_CONNECTIONS=False


# Airflow Init

_AIRFLOW_DB_UPGRADE=True

_AIRFLOW_WWW_USER_CREATE=True

_AIRFLOW_WWW_USER_USERNAME=airflow

_AIRFLOW_WWW_USER_PASSWORD=airflow

```

2. Aditionally, set the **required** environment variables for the database and the Docker volumes:

- ETL database:

`_POSTGRES_DB`

`_POSTGRES_USER`

`_POSTGRES_PASSWORD`

`_POSTGRES_HOST`

`_POSTGRES_PORT`


- Volumes:

`PG_DATA`

`JUPYTER_NOTEBOOKS`

  
#### Postgresql and jupyter

1. ``$ docker network create etl_network``

2. ``$ docker build -f jupyter-dockerfile .``

3. ``$ docker-compose --env-file ./.env -f ./postgres-docker-compose.yaml up -d``

4. ``$ docker ps`` to get the jupyter container ID.

5. ``$ docker logs jupyter_notebook`` to get the link with the auth token.

6. open jupyter lab on your browser.


#### Airflow

1. ``$ docker build -f airflow-dockerfile .``

2. ``$ docker-compose -f airflow-docker-compose.yaml up -d``


#### Clean Up

To stop and remove all the containers, including the bridge network, run the following command:

```

docker-compose -f postgres-docker-compose.yaml down --volumes --rmi all

docker-compose -f airflow-docker-compose.yaml down --volumes --rmi all

docker network rm etl_network

```