# bku-neo4j-movie-recommendation

## Bước 1:  
Tạo folder neo4j tại home gồm ba folder con gồm import, conf, data

## Bước 2:
Download và đặt các file csv vào folder import

## Bước 3: 
Đặt file neo4j.conf vào folder conf với nội dung sau:

```Conf
dbms.memory.transaction.total.max=0
dbms.memory.heap.initial_size=12G
dbms.memory.heap.max_size=12G
```

## Bước 4

Chạy theo thứ tự các command sau:

```Cypher
docker run -d --name testneo4j --publish=7474:7474 --publish=7687:7687 --volume=$HOME/neo4j/data:/data --volume=$HOME/neo4j/import:/import --volume=$HOME/neo4j/conf:/conf -e NEO4J_apoc_export_file_enabled=true -e NEO4J_apoc_import_file_enabled=true -e NEO4J_apoc_import_file_use__neo4j__config=true -e NEO4J_PLUGINS=\[\"apoc\"\] neo4j
``` 

```Cypher
docker exec --interactive --tty testneo4j neo4j-admin database import full --nodes=Movie=/import/movies.csv --nodes=User=/import/users.csv --nodes=GenomeTag=/import/genome-tags.csv --relationships=RATED=/import/ratings.csv --relationships=SCORED=/import/genome-scores.csv --overwrite-destination neo4j
```

Create index
```Cypher
CREATE INDEX user_range_index_id FOR (n:User) ON (n.userId);
CREATE  INDEX movie_range_index_id FOR (n:Movie) ON (n.movieId);
CREATE INDEX rated_range_index_rating FOR ()-[r:RATED]-() ON (r.rating);
```


