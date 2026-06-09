# Clickhouse::Activerecord

A Ruby database ActiveRecord driver for ClickHouse. Support Rails >= 7.1.

## Installation

```ruby
gem 'clickhouse-activerecord'
```

    $ bundle

## Connection Parameters

```yml
default: &default
  adapter: clickhouse
  database: database
  host: localhost
  port: 8123
  username: username
  password: password
  http_auth: query_params # optional: query_params, basic, x_clickhouse_headers
  ssl: true
  debug: true
  migrations_paths: db/clickhouse
  cluster_name: 'cluster_name'
  replica_name: '{replica}'
  read_timeout: 300
  write_timeout: 300
  keep_alive_timeout: 300
```

## Usage in Rails

```ruby
class Action < ActiveRecord::Base
end
```

Materialized view:
```ruby
class ActionView < ActiveRecord::Base
  self.is_view = true
end
```

Second database:
```ruby
class Action < ActiveRecord::Base
  establish_connection :clickhouse
end
```

## Rake Tasks

```
$ rake db:create
$ rake db:drop
$ rake db:purge
$ rake db:reset
$ rake db:migrate
$ rake db:rollback
$ rails g clickhouse_migration MIGRATION_NAME COLUMNS
```

Multiple databases:
```
$ rake db:create:clickhouse
$ rake db:schema:dump:clickhouse
$ rake db:schema:load:clickhouse
$ rake clickhouse:schema:dump -- --simple
$ rake clickhouse:structure:dump
$ rake clickhouse:structure:load
```

## Testing

RSpec:
```ruby
require 'clickhouse-activerecord/rspec'
```

Minitest:
```ruby
require 'clickhouse-activerecord/minitest'
```

## Local Development

```bash
docker compose -f .docker/docker-compose.yml up -d
bin/test-single

docker compose -f .docker/docker-compose.cluster.yml up -d
CLICKHOUSE_PORT=28123 CLICKHOUSE_CLUSTER=test_cluster bin/test-single
```
