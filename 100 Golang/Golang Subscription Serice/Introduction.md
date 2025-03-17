
Import postgres.

go get 

go get github.com/jackc/pgconn
go get github.com/jackc/pgx/v4
go get github.com/alexedwards/scs/v2
go get github.com/alexedwards/scs/redisstore
go get github.com/go-chi/chi/v5

Put the docker file at root.

version: '3'

services:

  #start a psql, ensure that data is stored to a mounted value
  postgres:
    image: 'postgres:14.2'
    ports:
      - "5432:5432"
    restart: always
    environment:
      - POSTGRES_USER: postgres
      - POSTGRES_PASSWORD: password
      - POSTGRES_DB: concurrency
    volumes:
      - ./db-data/postgres/:/var/lib/postgresql/data/


  #start redis and ensure that its mounted
  redis:
    image: 'redis-alpine'
    ports:
      - "6379:6379"
    restart: always
    volumes:
      - ./db-data/redis/:/data

  #start mailhog
  mailhog:
    image: 'mailhog/mailhog:latest'
    ports:
      - "1025:1025"
      - "8025:8025"
    restart: always

run this 
using

docker-compose up -d

-d is for detached mode, runs in background.

Setting up the [[postgres connection in golang]]
