Mark Shust's Docker Configuration for Lumen

## Docker Hub

View Dockerfiles:

- [markoshust/lumen-php-apache (Docker Hub)](https://hub.docker.com/r/markoshust/lumen-php-apache/)
	- 7.2
		- [`latest`, `7.2`, `7.2-0`](https://github.com/markoshust/docker-lumen/tree/master/images/php-apache/7.2)

## Usage

This configuration is intended to be used as a Docker-based development environment for Lumen.

Folders:

- `images`: Docker images
- `compose`: sample setups with Docker Compose

## Setup a New Lumen Project

1. Setup a new project sekeleton from this repository:

```
mkdir myapp && cd $_
git init
git remote add origin git@github.com:markoshust/docker-lumen.git
git fetch origin
git checkout origin/master -- compose
mv compose/* .
rm -rf compose .git
git init
```

2. Setup your ip loopback for proper IP resolution with Docker: `./bin/initloopback`

3. Add an entry to `/etc/hosts` with your custom domain: `10.254.254.254 myapp.test` (assuming the domain  you want to setup is `myapp.test`). Be sure to use a `.test` tld, as `.localhost` and `.dev` will present issues with domain resolution.

4. Start your Docker containers with: `./bin/start`.

5. Download Lumen within the running Docker container with composer: `./bin/composer create-project --prefer-dist laravel/lumen .`

6. You may now access your site at `http://myapp.test:8000` (or whatever domain you setup).

## Existing Lumen Project Setup

Your source code should go in the `src` folder, and you can then kick your project off with `./bin/start`.

## Custom CLI Commands

- `./bin/bash`: Drop into the bash prompt of your Docker container. The `phpfpm` container should be mainly used to access the filesystem within Docker.
- `./bin/cli`: Run any CLI command without going into the bash prompt. Ex. `./bin/cli ls`
- `./bin/composer`: Run the composer binary. Ex. `./bin/composer install`
- `./bin/initloopback`: Setup your ip loopback for proper Docker ip resolution.
- `./bin/start`: Start the Docker Compose process and your app. Ctrl+C to stop the process.

## Misc Info

### Database

- The hostname of each service is the name of the service within the `docker-compose.yml` file. So for example, MySQL's hostname is `db` (not `localhost`) when accessing it from a Docker container.

### Composer Authentication

The `~/.composer` directory is bind-mounted to the container as a volume, so you can setup your `~/.composer/auth.json` file on the host with the following contents, like so:

```
{
    "http-basic": {
        "your-repo.com": {
            "username": "YOUR_USERNAME",
            "password": "YOUR_PASSWORD"
        }
    }
}
```

...then, your authentication credentials will also work within the running Docker container.
