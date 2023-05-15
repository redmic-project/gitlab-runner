# GitLab Runner

Deployment of a service to run GitLab CI/CD workloads

## Deployment

First time, you must deploy `gitlab-runner-registrar` service to configure and register a new runner instance. This action creates a configuration file into a shared volume, ready for `gitlab-runner` to use.

Then, deploy `gitlab-runner` (launching default-named deployment jobs) to start receiving jobs to run.

## Delete old runners

Service will try to unregister old version of same runner being registered, before creating the new one.

When runners name change, you must delete old registrations manually. Go to a running `gitlab-runner` container shell and execute one of the following commands:

```sh
# Remove one runner, by GitLab instance URL and runner token
gitlab-runner unregister --url http://gitlab.example.com/ --token t0k3n

# Remove one runner, by runner name
gitlab-runner unregister --name test-runner

# Remove only broken runners
gitlab-runner verify --delete

# Remove all runners
gitlab-runner unregister --all-runners
```

## Environment isolation

This service is configured to work with `docker` executor. By default, your CI jobs will run in a Docker container launched at the host system of `gitlab-runner` service, sharing the same Docker daemon and resources (networks, volumes...).

In some use cases this is a desired behaviour, because you need local resources from host or it's local network.

To achieve isolation from host's Docker environment, you should use `dind` (docker-in-docker) as a service, applied to your CI job at `.gitlab-ci.yml`. This way, launched container cannot see resources at host system.
