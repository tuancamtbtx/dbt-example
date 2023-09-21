# Airflow + dbt Core

This repository provides an example Airflow DAG (Directed Acyclic Graph) that runs dbt Core commands to transform data. 

## Prerequisites

Before you begin, you'll need the following:

- Airflow installed and configured
- dbt Core installed and configured
- A database or data warehouse to transform data in

## Installation

1. Clone this repository to your local machine:

   ```
   git clone https://github.com/tuancamtbtx/airflow-dbt-core.git
   ```

2. Add your dbt Core project and profile directories to the `docker-compose.yaml` file:

   ```
   version: '3'
   services:
     postgres:
       image: postgres:13
       environment:
         POSTGRES_DB: mydb
         POSTGRES_USER: myuser
         POSTGRES_PASSWORD: mypassword
       ports:
         - 5432:5432
     scheduler:
       build:
         context: .
         dockerfile: Dockerfile.scheduler
       depends_on:
         - postgres
       volumes:
         - ./dags:/usr/local/airflow/dags
         - /path/to/dbt/project:/usr/local/dbt/project
         - /path/to/dbt/profiles:/usr/local/dbt/profiles
   ```

3. Build the Airflow scheduler Docker image:

   ```
   docker-compose build scheduler
   ```

## Usage

1. Start the Airflow scheduler:

   ```
   docker-compose up scheduler
   ```

2. Open the Airflow web UI in your browser:

   ```
   http://localhost:8080
   ```

3. Create a new DAG by clicking the "Create" button in the top menu:

   ```
   DAG name: my_dbt_dag
   ```
   
4. Add dbt Core operators to the DAG by clicking the "+" button in the DAG editor:

   ```
   Task ID: dbt_seed
   Operator: dbt.operators.dbt.DbtSeedOperator
   Arguments:
     project_dir: /usr/local/dbt/project
     profiles_dir: /usr/local/dbt/profiles
     target: my_target_name

   Task ID: dbt_run
   Operator: dbt.operators.dbt.DbtRunOperator
   Arguments:
     project_dir: /usr/local/dbt/project
     profiles_dir: /usr/local/dbt/profiles
     target: my_target_name

   Task ID: dbt_test
   Operator: dbt.operators.dbt.DbtTestOperator
   Arguments:
     project_dir: /usr/local/dbt/project
     profiles_dir: /usr/local/dbt/profiles
     target: my_target_name
   ```

5. Add any necessary dependencies between the dbt Core operators and other operators in the DAG.

6. Save the DAG and trigger it to run using the Airflow UI or the `airflow trigger_dag` command.

## Cleanup

To stop the Airflow scheduler and remove the Docker containers, run the following command:

```
docker-compose down
```

This will stop the scheduler and remove any associated resources.

## Troubleshooting

If you encounter any issues during the installation or usage of Airflow and dbt Core, please refer to the [Airflow documentation](https://airflow.apache.org/docs/) or the [dbt Core documentation](https://docs.getdbt.com/docs/) for troubleshooting tips and guidance.