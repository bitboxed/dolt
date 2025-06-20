## Development

This serves as a preliminary document for detailing local compilation of dolt for [Bitnode](https://github.com/bitboxed/bitnode/) purposes.

### Building dolt locally
You'll need Go 1.21+. Installation on Mac is like:
```
brew install go

# Add to ~/.zshrc:
# Correct path for Homebrew on Apple  Silicon 
export GOROOT="/opt/homebrew/opt/go/libexec"
export GOPATH="$HOME/go" 
export PATH="$GOPATH/bin:$GOROOT/bin:$PATH"

# then:
source ~/.zshrc

# check go version
go version
go version go1.24.4 darwin/arm64
```

Build dolt:
```
cd /go
go mod tidy

cd /cmd/dolt
go build -o dolt
```

Test you can see the standard dolt interface w/ your compiled `dolt` binary:
```
./dolt --help

Valid commands for dolt are
                init - Create an empty Dolt data repository.
              status - Show the working tree status.
                 add - Add table changes to the list of staged table changes.
                diff - Diff a table.
                ...

$ ./dolt sql-server --help
NAME
        dolt sql-server - Start a MySQL-compatible server.
SYNOPSIS
        dolt sql-server --config <file>
        dolt sql-server [-H <host>] [-P <port>] [-t <timeout>] [-l <loglevel>] [--data-dir <directory>] [-r]
        ...
```


### Building an arm64 image of dolt
You can use the sample [Dockerfile.dolt](./Dockerfile.dolt) to compile and tag a dolt image.
```
# test
docker buildx build --platform linux/arm64 -f Dockerfile.dolt --load .

# tag and publish
docker buildx build --platform linux/arm64 -f Dockerfile.dolt -t <your-user>/dolt:arm64 --push .
```

### Remotesrv
You can see similar steps documented here for compiling and publishing `remotesrv` added in this [README](./go/utils/remotesrv/README.md).