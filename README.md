# docker-config

For local development, create a `docker-compose.override.yml` with contents like the sample file. This includes exposing the http port, not running with TLS, and pointing at local files to build images.

To deploy in production, use the prod override file thusly:

  docker-compose -f docker-compose.yml -f docker-compose.prod.override.yml up --detach

Remove the --detach if you want to run in the foreground and see the logs.

See https://docs.docker.com/compose/extends/#multiple-compose-files for much more information.
