# Cutie - web

## Technology:
- Nginx - aggregator reverse proxy, docker based
- Supports HTTPS with a self-signed certificate
- Routes to components Cutie : [`web-site`](https://github.com/blank-org/cutie-web-site) - docker based

## Localhost - hosts file

- local.avyaan.tech - 127.0.0.1

## Project structure

```
Web
│
├── Site
│   └── Project             (avyaan-tech/web-site)
|       ├── root\framework  (blank-org/cutie - submoduled)
│       ├── interim
│       └── public
│
├── Project                 (avyaan-tech/web * this repo)
│   ├── interim             (avyaan-tech/web-interim)
│   └── public              (avyaan-tech/web-public > avyaan.tech)
|
│
└── Tiggu                   (blank-org/tiggu)
│
└── Firebase                (blank-org/firebase)
```

## Certificate

For SSL generate `server.key` & `server.crt` files

cert_details.txt :

```
[req]
distinguished_name = req_distinguished_name
prompt = no

[req_distinguished_name]
CN = local.avyaan.tech
```

Generate self-signed certificate
```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.crt -config cert_details.txt
```

You may also want to add this to your trusted-root CA store  
\- so that you are not presented with the *insecure origin* message when navigating to `local.avyaan.tech`


## Manage
`Firebase` : docker based - project is to provide firebase CLI.  
Used to setup CI; Not required afterwards - hence separate.

## Website
`avyaan.tech` \> mapped to `public` directory

## Setup
- Run following script to setup the directory structure & repos
```bash
base_directory="web"

# function to create directory and clone git repository
clone_repo() {
    local path=$1
    local remote_url=$2
    local full_path="$base_directory/$path"

    # create base directory if it doesn't exist
    mkdir -p "$base_directory"
    
    # clone the repository into the specified path
    git clone --recurse-submodules "$remote_url" "$full_path"
}

# clone git repositories into specific paths
clone_repo "site/project" "https://github.com/avyaan-technologies/web-site.git"
clone_repo "project" "https://github.com/avyaan-technologies/web.git"
clone_repo "project/interim" "https://github.com/avyaan-technologies/web-interim.git"
clone_repo "project/public" "https://github.com/avyaan-technologies/web-public.git"
clone_repo "tiggu" "https://github.com/blankorg/cutie-tiggu.git"
# clone_repo "firebase" "https://github.com/blank-org/firebase.git"
```

- Then build and run docker for `web/project`

```bash
docker compose -f compose-dev.yaml up -p avyaan-web

```
