## Local WordPress Development

For video tutorials on setup:

- [Local WordPress Setup in Five Minutes](https://www.youtube.com/watch?v=gEceSAJI_3s)
- [Adding SSL support to your WordPress Docker Setup](https://www.youtube.com/watch?v=HH4s3x1PiA4)

## Setup

You will need [Docker Desktop](https://www.docker.com/products/docker-desktop/) and [mkcert](https://github.com/FiloSottile/mkcert) installed.

Clone this repository to your local machine if you haven't already done so.

`git clone git@github.com:southseaboy/local-wordpress-development.git`

Copy the environment file then edit it - Fill in the variables to match something like below. NB Ctrl-X then Y to quit the editor when done.

```
cp env.example .env
nano .env
```

example environment variables
```bash
HOSTNAME=host.docker.internal

CONTAINER_NAME=my_app

DATABASE_NAME=wordpress
DATABASE_USER=admin
DATABASE_PASSWORD=test
DATABASE_ROOT_PASSWORD=test
```

### Create certs

`cd` into the `nginx` directory and run `mkcert --install` to finish the setup of `mkcert`. You should **not** have to run this command again. You will need to restart your browsers to get the new certificate recognized in the certificate store.

`cd` into the `certs` directory and run `mkcert host.docker.internal` which should create two files called `host.docker.internal-key.pem` and `host.docker.internal.pem`.

Now cd back upto the `local-wordpress-development` directory and run `docker compose up --build` to build the image and bring it online. You should be able to access your site at `host.docker.internal` and see the WordPress install screen. If you want to keep the docker process running in the background (without terminal logging all files accessed) run `docker compose up --build -d`.

### PHPMyAdmin

This is available at `host.docker.internal:8081` with the `DATABASE_USER` and `DATABASE_PASSWORD` you set in your `.env` file

## Linux Users

You will have issues trying to edit the files in your `wp-content` directory to do any development. macOS and Windows have virtualisation layers that make this not a problem on those platforms. To fix this on linux you need to use the `setfacl` command changing `<localuser>` to the local user you're logged in under.

```bash
sudo setfacl -Rm "u:<localuser>:rwx,d:u:<localuser>:rwx" ./src
```

This command sets up your local user as someone that owns and can edit the files in addition to the Docker user
