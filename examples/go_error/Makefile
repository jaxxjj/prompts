NAME=loyalty_points
TEST_DIRS := $(shell go list ./...)
REDIS_DOCKER_NAME=$(NAME)-redis
REDIS_PORT=6379
POSTGRES_DOCKER_NAME=$(NAME)-postgres
POSTGRES_PASSWORD=my-secret
POSTGRES_DB=$(NAME)_test_db
POSTGRES_PORT=5432
POSTGRES_APPNAME=$(NAME)_test
ZOOKEEPER_DOCKER_NAME=$(NAME)-zookeeper
ZOOKEEPER_PORT=32181
KAFKA_DOCKER_NAME=$(NAME)-kafka
KAFKA_PORT=29092
KAFKA_BROKERADDRS="localhost:$(KAFKA_PORT)"

test: submodule
	export ENV=test && \
	export POSTGRES_APPNAME=$(APPNAME) && \
	export KAFKA_BROKERADDRS=$(KAFKA_BROKERADDRS) && \
	go test ${TEST_DIRS} -test.v -p 1

test-golden-update-svc update-golden:
	export ENV=test && \
	export POSTGRES_APPNAME=$(APPNAME) && \
	export KAFKA_BROKERADDRS=$(KAFKA_BROKERADDRS) && \
	go test ./pkg/service/loyaltypointsv1 -test.v -p 1 -update

codecov: submodule
	export ENV=test && \
	export POSTGRES_APPNAME=$(APPNAME) && \
	export KAFKA_BROKERADDRS=$(KAFKA_BROKERADDRS) && \
	go test ./pkg/service/loyaltypointsv1 -coverprofile=coverage.txt -covermode=atomic -p 1

submodule:
	git submodule update --init --recursive

build: submodule
	CGO_ENABLED=1 GOOS=linux GOARCH=amd64 go build -o bin/app ./cmd/main.go

build-cron: submodule
	CGO_ENABLED=1 GOOS=linux GOARCH=amd64 go build -o bin/app-cron ./pkg/service/cronjob/cmd/cronjob.go

mock:
	mockery --srcpkg github.com/NFTGalaxy/galxe-campaign/api/go --name CampaignServiceClient --output pkg/mocks
	mockery --srcpkg github.com/NFTGalaxy/galxe-space/api/go/galxe-space/followpb/v1 --name FollowServiceClient --output pkg/mocks
	mockery --srcpkg github.com/NFTGalaxy/galxe-user/api/go/client/userrpc --name UserClient --output pkg/mocks
	mockery --srcpkg github.com/NFTGalaxy/galxe-user/api/go/client/authrpc --name AuthClient --output pkg/mocks
	mockery --dir pkg/service/loyaltypointsv1 --name UploadDataI --output pkg/mocks

proto:
	cd api/proto && \
        buf generate

repo:
	cd ./pkg/repos && sqlc generate

gen-error:
	cd pkg/internal/galaxy-errors && make errkeygen

format-sql:
	find pkg/repos -type f -name "*.sql" -print0 | xargs -0 -I{} sql-formatter --fix -l postgresql {}

docker-postgres-start:
	docker run -d --name $(POSTGRES_DOCKER_NAME) -e POSTGRES_PASSWORD=$(POSTGRES_PASSWORD) -e POSTGRES_DB=$(POSTGRES_DB) -p $(POSTGRES_PORT):5432 postgres:14.5

docker-postgres-stop:
	docker stop $(POSTGRES_DOCKER_NAME)
	docker rm $(POSTGRES_DOCKER_NAME)

docker-redis-start:
	docker run -d --name $(REDIS_DOCKER_NAME) -p $(REDIS_PORT):6379 redis

docker-redis-stop:
	docker stop $(REDIS_DOCKER_NAME)
	docker rm $(REDIS_DOCKER_NAME)

docker-zookeeper-start:
	docker run -d --name $(ZOOKEEPER_DOCKER_NAME) -e ZOOKEEPER_CLIENT_PORT=$(ZOOKEEPER_PORT) -e ZOOKEEPER_TICK_TIME=2000 -p $(ZOOKEEPER_PORT):$(ZOOKEEPER_PORT) confluentinc/cp-zookeeper:latest

docker-zookeeper-stop:
	docker stop $(ZOOKEEPER_DOCKER_NAME)
	docker rm $(ZOOKEEPER_DOCKER_NAME)

docker-kafka-start:
	docker run -d --name $(KAFKA_DOCKER_NAME) -e KAFKA_BROKER_ID=1 -e KAFKA_ZOOKEEPER_CONNECT=host.docker.internal:$(ZOOKEEPER_PORT) -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:$(KAFKA_PORT) -e KAFKA_LISTENER_SECURITY_PROTOCOL_MAP=PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT -e KAFKA_INTER_BROKER_LISTENER_NAME=PLAINTEXT -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 -p $(KAFKA_PORT):$(KAFKA_PORT) confluentinc/cp-kafka:latest

docker-kafka-stop:
	docker stop $(KAFKA_DOCKER_NAME)
	docker rm $(KAFKA_DOCKER_NAME)

test-start-all: docker-postgres-start docker-redis-start docker-zookeeper-start docker-kafka-start
test-stop-all: docker-postgres-stop docker-redis-stop docker-zookeeper-stop docker-kafka-stop

.PHONY: lint lint-fix
lint:
	@echo "--> Running linter"
	@golangci-lint run

lint-fix:
	@echo "--> Running linter auto fix"
	@golangci-lint run --fix

local-run:
	export CGO_ENABLED=0 && \
	export BACKEND_ADDR="localhost:9090" && \
	export GRPC_PORT=9098 && \
	export HEALTHCHECK_PORT=8088 && \
	export METRIC_PORT=4015 && \
	export POSTGRES_DBNAME="loyalty_points_test_db_2" && \
	export POSTGRES_APPNAME=$(APPNAME) && \
	export KAFKA_BROKERADDRS=$(KAFKA_BROKERADDRS) && \
	export LOYALTY_POINTS_DISABLEOLDISSUEPOINTSFUNC=false && \
	export LOYALTY_POINTS_DOUBLEWRITING=true && \
	go run --mod=mod cmd/main.go

.PHONY: test build mock proto submodule gen-sqlc gen-error