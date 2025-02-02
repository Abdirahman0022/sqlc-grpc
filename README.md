## sqlc-grpc

Create a gRPC Server from the generated code by the awesome [sqlc](https://sqlc.dev/) project.

### Dependencies

- Go 1.16 or superior
- [protoc](https://github.com/protocolbuffers/protobuf/releases)
- sqlc, protoc-gen-go and protoc-gen-go-grpc

```sh
go install github.com/kyleconroy/sqlc/cmd/sqlc@latest
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

### Installation

```sh
go install github.com/walterwanderley/sqlc-grpc@latest
```

### Example

1. Create a queries.sql file:

```sql
--queries.sql

CREATE TABLE authors (
  id   BIGSERIAL PRIMARY KEY,
  name text      NOT NULL,
  bio  text,
  created_at TIMESTAMP
);

-- name: GetAuthor :one
SELECT * FROM authors
WHERE id = $1 LIMIT 1;

-- name: ListAuthors :many
SELECT * FROM authors
ORDER BY name;

-- name: CreateAuthor :one
INSERT INTO authors (
  name, bio, created_at
) VALUES (
  $1, $2, $3
)
RETURNING *;

-- name: DeleteAuthor :exec
DELETE FROM authors
WHERE id = $1;

```

2. Create a sqlc.yaml file

```yaml
version: "1"
packages:
  - name: "db"
    path: "internal/db"
    queries: "./queries.sql"
    schema: "./queries.sql"
    engine: "postgresql"

```

3. Execute sqlc

```sh
sqlc generate
```

4. Execute sqlc-grpc

```sh
sqlc-grpc -m "my/module/path"
```

5. Enjoy!
