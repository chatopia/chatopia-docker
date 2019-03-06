# docker-config

The `docker-compose.yml` file contains the production configuration. For local development, create a `docker-compose.override.yml` with contents like the sample file. This includes exposing the http port, not running with TLS, and pointing at local files to build images.

See https://docs.docker.com/compose/extends/#multiple-compose-files for much more information.
