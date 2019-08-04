# Install
Start by copying:
* elasticsearch.yml.template => elasticsearch.yml
* kibana.yml.template => kibana.yml

Run Elasticsearch and Kibana:
```
docker-compose up
```

Enter the elasticserach container:
```
docker-compose exec elasticsearch bash
```

Run the following lines inside the container. When prompted for anything just hit enter.
```
bin/elasticsearch-certutil ca
bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12
```

Press CTRL+D to exit the container then the run:
```
docker cp "$(docker-compose ps -q elasticsearch)":/usr/share/elasticsearch/elastic-certificates.p12 .
docker cp "$(docker-compose ps -q elasticsearch)":/usr/share/elasticsearch/elastic-stack-ca.p12 .
```

Now, stop both services using CTRL+C.
Then modify the following files:
* docker-compose.yml: uncomment lines. 
* elasticsearch.yml: uncomment lines and set xpack.security.enabled to true.

Start the services again:
```
docker-compose up
```

Enter the elasticserach container again:
```
docker-compose exec elasticsearch bash
```

Inside the container run:
```
bin/elasticsearch-setup-passwords auto
```

Write down the passwords somewhere safe and exit the container by pressing CTRL+D.
Now, stop the both services again using CTRL+C.
Modify the following files:
* kibana.yml: uncomment lines and enter kibana password. 

Finally, start the services one last time:
```
docker-compose up -d
```

Open http://localhost:5601 and use elastic as user and the password for the elastic user.

Create two new roles:
* Role: manager, index: estate, priveleges: all
* Role: visitor, index: estate, priveleges: read

Create two new users:
* Username: manager, role: manager
* Username: visitor, role: visitor
