# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

install:
	pip install poetry
	poetry install -E pyarrow -E hive -E s3fs -E glue -E adlfs -E duckdb

check-license:
	./dev/check-license

lint:
	poetry run pre-commit run --all-files

test:
	poetry run coverage run --source=pyiceberg/ -m pytest tests/ -m unmarked ${PYTEST_ARGS}
	poetry run coverage report -m --fail-under=90
	poetry run coverage html
	poetry run coverage xml

test-s3:
	sh ./dev/run-minio.sh
	poetry run coverage run --source=pyiceberg/ -m pytest tests/ -m s3 ${PYTEST_ARGS}

test-integration:
	docker-compose -f dev/docker-compose-integration.yml kill
	docker-compose -f dev/docker-compose-integration.yml build
	docker-compose -f dev/docker-compose-integration.yml up -d
	sleep 20
	poetry run coverage run --source=pyiceberg/ -m pytest tests/ -m integration ${PYTEST_ARGS}

test-adlfs:
	sh ./dev/run-azurite.sh
	poetry run coverage run --source=pyiceberg/ -m pytest tests/ -m adlfs ${PYTEST_ARGS}
