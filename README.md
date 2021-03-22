# postal-docker 


### `akhil/postal-base` image is built from `base-image/Dockerfile`

The base image as all the gemfile dependencies installed.

`postal` command is located at `/opt/postal/app/postal`

`Procfile` in `postal-base` contains all the components. But in this step `web` and the rest are being separated to scale `web` independently.

Postal has the following components

1. Web interface (puma) has the admin UI and the `api`. Run on port `5000` by default. Setup an nginx or caddy or some other reverse proxy.

2. Rest (workers, fast_server for click tracking, smtp_server)

create `postal.yml` use `postal.example.yml` for reference

- generate 128 size secret key using `src/create-secretkey.sh` and paste it in postal.yml. It is unique to the installation.

- run `scripts/create-user.sh` (wrapper for `postal make-user`) to setup initial admin.


To get `IP pools` to work after enabling in the `postal.yml` file and adding IPs using the admin ui you may have to go to `postal console` and check `Server.first` if the ip_pool is not null

https://github.com/postalhq/postal/issues/1319
