# Configuration
Each of RoadRunner service requires proper configuration. By default, such configuration is merged into one file which must be located in a root of your project. Each service configuration is located under the designated section. The config file must be named as `.rr.{format}` where format is `yml`, `json` and others supported by `sp13f/viper`.

Sample configuration:

```yaml
# defines environment variables for all underlying php processes
env:
  key: value

# rpc bus allows php application and external clients to talk to rr services.
rpc:
  # enable rpc server (enabled by default)
  enable: true

  # rpc connection DSN. Supported TCP and Unix sockets.
  listen:     tcp://127.0.0.1:6001

# http service configuration.
http:
  # http host to listen.
  address:   0.0.0.0:8080

  ssl:
    # custom https port (default 443)
    port:     443

    # force redirect to https connection
    redirect: true

    # ssl cert
    cert:     server.crt

    # ssl private key
    key:      server.key

  # max POST request size, including file uploads in MB.
  maxRequest: 200

  # file upload configuration.
  uploads:
    # list of file extensions which are forbidden for uploading.
    forbid: [".php", ".exe", ".bat"]

  # http worker pool configuration.
  workers:
    # php worker command.
    command:  "php psr-worker.php pipes"

    # connection method (pipes, tcp://:9000, unix://socket.unix). default "pipes"
    relay:    "pipes"

    # worker pool configuration.
    pool:
      # number of workers to be serving.
      numWorkers: 4

      # maximum jobs per worker, 0 - unlimited.
      maxJobs:  0

      # for how long worker is allowed to be bootstrapped.
      allocateTimeout: 60

      # amount of time given to worker to gracefully destruct itself.
      destroyTimeout:  60

# static file serving. remove this section to disable static file serving.
static:
  # root directory for static file (http would not serve .php and .htaccess files).
  dir:   "public"

  # list of extensions for forbid for serving.
  forbid: [".php", ".htaccess"]
```

Minimal version:

```yaml
http:
  address: :8080
  workers:
    command: "php psr-worker.php"
    pool:
      numWorkers: 4
```

## Console flags
You can overwrite any of the config values using `-o` flag:

```
rr serve -v -d -o http.address=:80 -o http.workers.pool.numWorkers=1
```