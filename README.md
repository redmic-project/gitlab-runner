# GitLab Runner

Deployment of a service to run GitLab CI/CD workloads

## Deployment

At first time, you must deploy `gitlab-runner-registrar` to configure and register a new runner instance. Then, deploy `gitlab-runner` (launching default deployment jobs) to receive jobs to run.

You must tell to your GitLab instance to use Docker in TLS mode before launching your jobs. Set `DOCKER_HOST=tcp://docker:2376` into your parent group or project.

## Delete old runners

Service will try to unregister old version of same runner being registered, before creating the new one.

When runners name change, you must delete old registrations manually. Go to a running gitlab-runner container shell and execute one of the following commands:

```sh
# Remove one runner, by GitLab instance URL and runner token
gitlab-runner unregister --url http://gitlab.example.com/ --token t0k3n

# Remove one runner, by runner name
gitlab-runner unregister --name test-runner
￼
# Remove only broken runners
gitlab-runner verify --delete

# Remove all runners
gitlab-runner unregister --all-runners
```
