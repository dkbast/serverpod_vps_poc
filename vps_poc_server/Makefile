.PHONY: generate run reset help

.DEFAULT_GOAL := help

help: ## show this help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

generate: ## generate updated models and create migrations
	serverpod generate
	serverpod create-migration

run: ## run the server
	docker compose up --build --detach
	dart run bin/main.dart --apply-migrations

reset: ## reset the server
	rm -rf migrations
	serverpod generate
	serverpod create-migration
	docker compose down --volumes --remove-orphans
	docker compose up --build --detach
	dart run bin/main.dart --role maintenance --apply-migrations

