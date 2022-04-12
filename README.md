This is the code repo for dbt tutorial at https://www.startdataengineering.com/post/dbt-data-build-tool-tutorial

### Prerequisites

1. [Docker](https://docs.docker.com/get-docker/) and [Docker compose](https://docs.docker.com/compose/install/)
2. [dbt](https://docs.getdbt.com/dbt-cli/installation/)
3. [pgcli](https://www.pgcli.com/install)
4. [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## Clone repository
Clone the git repo and start the data warehouse docker container

```bash
git clone https://github.com/bilaschandra/dbt_local_project.git
cd dbt_local_project
docker compose up -d
```

## Run dbt (Command and output)

```bash
export DBT_PROFILES_DIR=$(pwd)
```
```bash
dbt snapshot
```
- Output
```bash
06:15:38  Running with dbt=1.0.4
06:15:38  Found 6 models, 10 tests, 1 snapshot, 0 analyses, 165 macros, 0 operations, 0 seed files, 3 sources, 0 exposures, 0 metrics
06:15:38  
06:15:38  Concurrency: 1 threads (target='dev')
06:15:38  
06:15:38  1 of 1 START snapshot snapshots.customers_snapshot.............................. [RUN]
06:15:38  1 of 1 OK snapshotted snapshots.customers_snapshot.............................. [SELECT 100 in 0.05s]
06:15:38  
06:15:38  Finished running 1 snapshot in 0.15s.
06:15:38  
06:15:38  Completed successfully
06:15:38  
06:15:38  Done. PASS=1 WARN=0 ERROR=0 SKIP=0 TOTAL=1
```

```bash
dbt run
```
- Output
```bash
06:15:41  Running with dbt=1.0.4
06:15:41  Found 6 models, 10 tests, 1 snapshot, 0 analyses, 165 macros, 0 operations, 0 seed files, 3 sources, 0 exposures, 0 metrics
06:15:41  
06:15:41  Concurrency: 1 threads (target='dev')
06:15:41  
06:15:41  1 of 6 START view model warehouse.stg_eltool__customers......................... [RUN]
06:15:41  1 of 6 OK created view model warehouse.stg_eltool__customers.................... [CREATE VIEW in 0.03s]
06:15:41  2 of 6 START view model warehouse.stg_eltool__orders............................ [RUN]
06:15:41  2 of 6 OK created view model warehouse.stg_eltool__orders....................... [CREATE VIEW in 0.02s]
06:15:41  3 of 6 START view model warehouse.stg_eltool__state............................. [RUN]
06:15:41  3 of 6 OK created view model warehouse.stg_eltool__state........................ [CREATE VIEW in 0.02s]
06:15:41  4 of 6 START table model warehouse.fct_orders................................... [RUN]
06:15:41  4 of 6 OK created table model warehouse.fct_orders.............................. [SELECT 999 in 0.03s]
06:15:41  5 of 6 START table model warehouse.dim_customers................................ [RUN]
06:15:41  5 of 6 OK created table model warehouse.dim_customers........................... [SELECT 100 in 0.02s]
06:15:41  6 of 6 START view model warehouse.customer_orders............................... [RUN]
06:15:41  6 of 6 OK created view model warehouse.customer_orders.......................... [CREATE VIEW in 0.02s]
06:15:41  
06:15:41  Finished running 4 view models, 2 table models in 0.25s.
06:15:41  
06:15:41  Completed successfully
06:15:41  
06:15:41  Done. PASS=6 WARN=0 ERROR=0 SKIP=0 TOTAL=6
```

```bash
dbt test
```
- Output
```bash
06:19:33  Running with dbt=1.0.4
06:19:33  Found 6 models, 10 tests, 1 snapshot, 0 analyses, 165 macros, 0 operations, 0 seed files, 3 sources, 0 exposures, 0 metrics
06:19:33  
06:19:33  Concurrency: 1 threads (target='dev')
06:19:33  
06:19:33  1 of 10 START test accepted_values_customer_orders_order_status__delivered__invoiced__shipped__processing__canceled__unavailable [RUN]
06:19:33  1 of 10 PASS accepted_values_customer_orders_order_status__delivered__invoiced__shipped__processing__canceled__unavailable [PASS in 0.03s]
06:19:33  2 of 10 START test assert_customer_dimension_has_no_row_loss.................... [RUN]
06:19:33  2 of 10 PASS assert_customer_dimension_has_no_row_loss.......................... [PASS in 0.01s]
06:19:33  3 of 10 START test not_null_customer_orders_customer_id......................... [RUN]
06:19:33  3 of 10 PASS not_null_customer_orders_customer_id............................... [PASS in 0.02s]
06:19:33  4 of 10 START test not_null_dim_customers_customer_id........................... [RUN]
06:19:33  4 of 10 PASS not_null_dim_customers_customer_id................................. [PASS in 0.01s]
06:19:33  5 of 10 START test not_null_stg_eltool__customers_customer_id................... [RUN]
06:19:33  5 of 10 PASS not_null_stg_eltool__customers_customer_id......................... [PASS in 0.01s]
06:19:33  6 of 10 START test source_not_null_warehouse_customers_customer_id.............. [RUN]
06:19:33  6 of 10 PASS source_not_null_warehouse_customers_customer_id.................... [PASS in 0.02s]
06:19:33  7 of 10 START test source_not_null_warehouse_orders_order_id.................... [RUN]
06:19:33  7 of 10 PASS source_not_null_warehouse_orders_order_id.......................... [PASS in 0.02s]
06:19:33  8 of 10 START test source_relationships_warehouse_orders_cust_id__customer_id__source_warehouse_customers_ [RUN]
06:19:33  8 of 10 PASS source_relationships_warehouse_orders_cust_id__customer_id__source_warehouse_customers_ [PASS in 0.02s]
06:19:33  9 of 10 START test source_unique_warehouse_orders_order_id...................... [RUN]
06:19:33  9 of 10 PASS source_unique_warehouse_orders_order_id............................ [PASS in 0.02s]
06:19:33  10 of 10 START test unique_customer_orders_order_id............................. [RUN]
06:19:34  10 of 10 PASS unique_customer_orders_order_id................................... [PASS in 0.02s]
06:19:34  
06:19:34  Finished running 10 tests in 0.29s.
06:19:34  
06:19:34  Completed successfully
06:19:34  
06:19:34  Done. PASS=10 WARN=0 ERROR=0 SKIP=0 TOTAL=10
```

```bash
dbt docs generate
```
- Output
```bash
06:20:15  Running with dbt=1.0.4
06:20:16  Found 6 models, 10 tests, 1 snapshot, 0 analyses, 165 macros, 0 operations, 0 seed files, 3 sources, 0 exposures, 0 metrics
06:20:16  
06:20:16  Concurrency: 1 threads (target='dev')
06:20:16  
06:20:16  Done.
06:20:16  Building catalog
06:20:16  Catalog written to /dbt_local_project/target/catalog.json
```

```bash
dbt docs serve
```
- Output
```bash
06:21:30  Running with dbt=1.0.4
06:21:30  Serving docs at 0.0.0.0:8080
06:21:30  To access from your browser, navigate to:  http://localhost:8080
06:21:30  
06:21:30  
06:21:30  Press Ctrl+C to exit.
127.0.0.1 - - [12/Apr/2022 12:21:30] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [12/Apr/2022 12:21:30] "GET /manifest.json?cb=1649744490987 HTTP/1.1" 200 -
127.0.0.1 - - [12/Apr/2022 12:21:30] "GET /catalog.json?cb=1649744490987 HTTP/1.1" 200 -
```

## Insert updates into source customer table, to demonstrate snapshot

```bash
pgcli -h localhost -U dbt -p 5432 -d dbt
# password is password1234
COPY warehouse.customers(customer_id, zipcode, city, state_code, datetime_created, datetime_updated) FROM '/input_data/customer_new.csv' DELIMITER ',' CSV HEADER;
\q
```

Run snapshot and create models again.

```
dbt snapshot
dbt run
```

You can log into the data warehouse to see the models.

```bash
pgcli -h localhost -U dbt -p 5432 -d dbt
# password is password1234
select * from warehouse.customer_orders limit 3;
\q
```

## Stop docker container

```bash
docker compose down
```

## All command together (go to project directory and run those commands)

```bash
export DBT_PROFILES_DIR=$(pwd)

docker compose up -d
dbt snapshot
dbt run
dbt test
dbt docs generate
dbt docs serve
docker compose down
```

## Folder structure
By default dbt will look for warehouse connections in the file ~/.dbt/profiles.yml. The DBT_PROFILES_DIR environment variable tells dbt to look for the profiles.yml file in the current working directory.

You can also create a dbt project using dbt init. This will provide you with a sample project, which you can modify.

In the simple_dbt_project folder you will see the following folders.

├── README.md
├── analysis
├── data
├── dbt_project.yml
├── docker-compose.yml
├── macros
├── models
│   ├── marts
│   │   ├── core
│   │   │   ├── core.yml
│   │   │   ├── dim_customers.sql
│   │   │   └── fct_orders.sql
│   │   └── marketing
│   │       ├── customer_orders.sql
│   │       └── marketing.yml
│   └── staging
│       ├── src_eltool.yml
│       ├── stg_eltool.yml
│       ├── stg_eltool__customers.sql
│       ├── stg_eltool__orders.sql
│       └── stg_eltool__state.sql
├── profiles.yml
├── raw_data
│   ├── customer.csv
│   ├── customer_new.csv
│   ├── orders.csv
│   └── state.csv
├── snapshots
│   └── customers.sql
├── tests
│   └── assert_customer_dimension_has_no_row_loss.sql
└── warehouse_setup
    └── init.sql

`analysis`: Any .sql files found in this folder will be compiled to raw sql when you run dbt compile. They will not be run by dbt but can be copied into any tool of choice.

`data`: We can store raw data that we want to be loaded into our data warehouse. This is typically used to store small mapping data.

`macros`: Dbt allows users to create macros, which are sql based functions. These macros can be reused across our project.
## Main repository (Thanks to [josephmachado](https://github.com/josephmachado))

Original repository is [simple_dbt_project](https://github.com/josephmachado/simple_dbt_project)
