## Django/postgres skeleton app overview

Two docker containers: `django` running django `pgdb` running postgres.

The site isn't doing anything interesting but when it's running it should be at http://127.0.0.1:8000

## Setup

In the "core" folder there is a file `example.env`.
Copy this into a new `.env` file and fill it in.
`DJANGO_SECRET` should be a value you get from running `python gen_random_key.py`
`DB_PASSWORD` can be whatever you want.

## Initial load of CT.gov data

1. Download `.zip` from [https://aact.ctti-clinicaltrials.org/snapshots]()
2. Unzip into `data` folder (`unzip <filename.zip> -d data`)
3. Start running the docker containers `docker-compose up`
4. Connect to postgres. `docker exec -it pgdb psql -U postgres`
5. You should see the `postgres=#` prompt.
6. Add ctgov schema to pSQL search path `alter role postgres in database aact set search_path = ctgov, public;`
7. Connect to the aact database: `\c aact`
8. If you run `\dt` you should see something like this...

```
                  List of relations
 Schema |            Name            | Type  |  Owner   
--------+----------------------------+-------+----------
 ctgov  | baseline_counts            | table | postgres
 ctgov  | baseline_measurements      | table | postgres
 ctgov  | brief_summaries            | table | postgres
 ctgov  | browse_conditions          | table | postgres
 ctgov  | browse_interventions       | table | postgres
 ctgov  | calculated_values          | table | postgres
 ctgov  | categories                 | table | postgres
 ctgov  | central_contacts           | table | postgres
 ctgov  | conditions                 | table | postgres
 ctgov  | countries                  | table | postgres
```

## Turn the docker containers on/off

	docker-compose up
	docker-compose down

## Other useful commands

### Docker

`docker images` - see your docker images

`docker ps` - see your currently running docker containers

### psql

Here's a useful psql command cheat sheet: [https://gist.github.com/Kartones/dd3ff5ec5ea238d4c546]()

## Connect to the containers

### django
`docker exec -it django /bin/bash`

### pgdb
`docker exec -it pgdb /bin/bash` (note: you can only access the db as the user postges so if you connect this way you may need to switch users)

or to go straight into psql `docker exec -it pgdb psql -U postgres`
