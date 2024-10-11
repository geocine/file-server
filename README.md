# Go Simple File Server (go-sfs)

A simple file server implemented in Go with authentication, rate limiting, and daemon mode support.

## Features

- Authentication using basic auth (username/password)
- Rate limiting to prevent abuse
- Daemon mode support
- HTTPS with self-signed certificates
- Secure file upload and download

## Configuration

Default configuration (./config/config.json):

```json
{
  "rateLimit": {
    "requestsPerSecond": 1,
    "burst": 5
  },
  "daemon": {
    "pidFile": "./config/pid",
    "logFile": "./config/log"
  },
  "port": 8080,
  "userFile": "./config/users.json",
  "storage": "./data",
  "certFolder": "./config/certs"
}
```

## Docker Usage

### Docker Compose Example

```yaml
version: '3'

services:
  go-sfs:
    image: mietzen/go-sfs
    container_name: go-sfs
    ports:
      - "8080:8080"
    volumes:
      - ./config:/config
      - ./data:/data
    environment:
      - LIMITER_REQUESTS_PER_SECOND=1
      - LIMITER_BURST=5
      - PORT=8080
```

Run with:

```bash
docker-compose up -d
```

### Adding a User (Docker)

```bash
docker exec go-sfs /go-sfs -u username
```

## Binary Usage

### Build

```bash
git clone https://github.com/mietzen/go-sfs.git
cd go-sfs
go build
go test
```

### Running the Server

```bash
./go-sfs
```

### Environment Variables

- USER_FILE=/config/users.json
- STORAGE=/data
- CERTS=/config/certs
- LIMITER_REQUESTS_PER_SECOND=1
- LIMITER_BURST=5
- PORT=8080

### Adding a User

```bash
./go-sfs -u username -p password
```

### Daemon Mode

```bash
./go-sfs -d
```

## API Endpoints

- Upload File: `PUT /upload/{path}`
- Download File: `GET /download/{path}`
- List Files: `GET /files`
- Delete File: `DELETE /delete/{path}`

### Examples

#### Upload

```bash
curl -u username:password -X PUT -T "local/file" http://localhost:8080/upload/remote/file
```

#### Download

```bash
curl -u username:password -OJ http://localhost:8080/download/path/to/file
```

#### List Files

```bash
curl -u username:password http://localhost:8080/files
```

#### Delete

```bash
curl -u username:password -X DELETE http://localhost:8080/delete/path/to/file
```

## License

MIT License - see the [LICENSE](./LICENSE) file for details.
