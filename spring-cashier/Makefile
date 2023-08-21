all: clean

clean:
	mvn clean

compile:
	mvn compile

run: compile
	mvn spring-boot:run

# Get jar file
build: 
	mvn package	


# Docker

docker-build: build
	docker build --platform=linux/amd64 -t spring-cashier .
	docker images

# MySQL DB

mysql:
	docker run --platform=linux/amd64 -d --network starbucks --name mysql -td -p 3306:3306 -e MYSQL_ROOT_PASSWORD=welcome mysql:8.0

mysql-shell:
	docker exec -it mysql bash