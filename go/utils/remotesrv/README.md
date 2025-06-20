# remotesrv

remotesrv is a dolt compatible remote server which implements the grpc remote chunkstore api, and a simple file storage server over http.

## Installation

Currently only installation from source is supported.  To install run 

    go install .
    
from the remotesrv directory


## Usage


#### synopsis

    remotesrv [--dir <directory>] [--http-port <PORT>] [--grpc-port <PORT>] [--admin-user <user>] [--admin-password <password>]
    
#### options

    -dir string
    	root directory where files will be stored to and served from
    
    -grpc-port
    	port on which the grpc server is running in order to serve the grpc remote chunkstore api (Default 50051)
    
    -http-port
    	port on which the http file server is running (Default 80)
    
    -admin-user, -admin-password
        admin user & password when using basic auth to provide to remotesrv for auth
    
      
## Using with dolt

In order to point the dolt cli to use this server you will need to add a remote that uses this server, or clone from this server

#### add remote

    dolt remote add <remote> http://localhost:<PORT>/<ORG>/<REPO>
   
#### clone

    dolt clone http://localhost:<PORT>/<ORG>/<REPO>

### Building locally
From this directory (`go/utils/remotesrv`):
```
go mod tidy
go build -o remotesrv main.go cscache.go
```

### Publishing to Dockerhub
From the root of the bitboxed/dolt repo:
```
docker buildx build --platform linux/arm64 -f Dockerfile.remotesrv -t <your-user>/remotesrv:arm64 --push .
```

You can run this after reclaim disk space used in Buildx cache:
```
docker buildx prune --all --force
```