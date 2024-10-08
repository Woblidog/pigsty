default: up view

up: view
	docker compose up -d

run:
	docker run --init --name pgadmin --restart always --detach --publish 8885:80 \
	-e PGADMIN_DEFAULT_EMAIL=admin@pigsty.cc -e PGADMIN_DEFAULT_PASSWORD=pigsty \
	dpage/pgadmin4

view:
	@echo "visit http://adm.pigsty  or  http://10.10.10.10:8885"
	@echo "cred: admin@pigsty.cc  /  pigsty"

log:
	docker logs -f pgadmin

info:
	docker inspect pgadmin | jq

stop:
	docker stop pgadmin;

clean:
	docker stop pgadmin; docker rm -f pgadmin

# load pigsty server list
conf:
	docker cp ~/.servers.json pgadmin:/tmp/servers.json ;
	docker cp ~/.pgpass pgadmin:/pgpass ;
	docker exec -u 0 -it pgadmin chown pgadmin /tmp/servers.json /pgpass
	docker exec -it pgadmin /venv/bin/python3 /pgadmin4/setup.py load-servers --user admin@pigsty.cc /tmp/servers.json

dump:
	docker exec -it pgadmin /venv/bin/python3 /pgadmin4/setup.py dump-servers --user admin@pigsty.cc /tmp/servers.json
	docker cp pgadmin:/tmp/servers.json /tmp/servers.json

pull:
	docker pull dpage/pgadmin4

rmi:
	docker rmi dpage/pgadmin4

save:
	mkdir -p /tmp/docker
	docker save dpage/pgadmin4 | gzip -9 -c > /tmp/docker/pgadmin4.tgz

load:
	cat /tmp/docker/pgadmin4.tgz | gzip -d -c - | docker load

.PHONY: default up run view log info stop clean conf dump pull rmi save load