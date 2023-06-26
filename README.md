# gitlab-ci-practice

## Install

```sh
cp .env.template .env
```
1. Setup `.env` with [.env.template](.env.template)

```sh
docker compose up -d
```
2. Run

```sh
# in gitlab container
grep 'Password:' /etc/gitlab/initial_root_password
Password: CgmfBfGdcLfdgH9OmId9YXo4rNyDBcRbVvFdgfuk0V8=
```
3. Check root password

Init process takes time, wait for a while - about 10 min

```sh
gitlab-rake "gitlab:password:reset[root]"
# wait about 10 minutes... (why?)
Enter password:
Confirm password:
Password successfully updated for user with username root.
```
If not found, manually reset root password (don't know why). Note that password should be more than 8 characters. If not, you will be waiting 10 minutes again.

- https://stackoverflow.com/a/71546291/4295499

```sh
# in gitlab-runner container
gitlab-runner register \
  --url http://gitlab-runner:4080 \
  --token glrt-K5WfXsQN78GHPdTVN5nu
```
4. Add runner in gitlab admin page(http://localhost:4080/admin), and register gitlab runner

```
Runtime platform                                    arch=arm64 os=linux pid=33 revision=b72e108d version=16.1.0
Running in system-mode.
Enter the GitLab instance URL (for example, https://gitlab.com/):
[http://gitlab-web:4080]: 
Verifying runner... is valid                        runner=o-1sLCpq4
Enter a name for the runner. This is stored only in the local config.toml file:
[gitlab-runner-1]: 
Enter an executor: custom, docker, ssh, docker+machine, kubernetes, docker-windows, parallels, shell, virtualbox, docker-autoscaler, instance: docker
Enter the default Docker image (for example, ruby:2.7): alpine:latest
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
 
Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml"
```
Enter runner options and done :)

```sh
gitlab-runner run
```
(Optional) run (if not run with [service](https://docs.gitlab.com/runner/commands/#service-related-commands))


```sh
ngrok http --domain={your_domain}.ngrok-free.app 4080
```
5. Run ngrok to receive webhook from GitHub

6. Add ngrok domain to GitHub webhook url

## Reference
- https://docs.gitlab.com/ee/install/docker.html#install-gitlab-using-docker-compose
