# Docker-Compose Files for Homelab

This repository contains Docker-Compose files used to manage and orchestrate various services within my homelab environment. Each configuration is tailored for efficient deployment, management, and scaling of containerized applications. Feel free to explore, use, and modify these files to suit your own homelab needs. Contributions and suggestions are welcome!

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Usage](#usage)
   - [Cloning the Repository](#cloning-the-repository)
   - [Directory Structure](#directory-structure)
   - [Starting Services](#starting-services)
4. [Secret Management](#secret-management)
   - [How It Works](#how-it-works)
   - [Prerequisites](#secret-management-prerequisites)
   - [Initialising a New Machine](#initialising-a-new-machine)
   - [Adding a New Service .env](#adding-a-new-service-env)
5. [Available Services](#available-services)
6. [Configuration](#configuration)
7. [Contributing](#contributing)
8. [License](#license)
9. [Contact](#contact)

## Introduction
This repository aims to simplify the setup and management of containerized services in a homelab environment using Docker Compose. Each service is defined in a separate compose file, making it easy to start, stop, and configure as needed.

## Prerequisites

Before using these Docker-Compose files, ensure you have the following installed:
- [Docker](https://www.docker.com/)
- [Docker-Compose](https://docs.docker.com/compose/)

## Usage

### Cloning the Repository
Clone the repository to your local machine using:
```bash
git clone https://github.com/andronics/homelab-docker-compose.git
cd homelab-docker-compose
```

### Directory Structure
The repository is organized as follows:

```
homelab-docker-compose/
├── audiobookshelf/
│   └── .env
│   └── compose.yml
├── service2/
│   └── compose.yml
└── README.md
```

### Starting Services

Navigate to the directory of the desired service. If available copy `.env.sample` to `.env` by executing:

```sh
$ cp '.env.sample' '.env'
```

```sh
$ docker compose up -d
```

This command starts the service in detached mode.


## Secret Management

All `.env` files in this repository are encrypted at rest using a custom `git-crypt` implementation built on top of OpenSSL AES-256-CBC. Git's clean/smudge filter mechanism transparently encrypts files on `git add` and decrypts them on `git checkout` — no manual steps required once the key is in place.

### How It Works

```
Working directory          Git object store
─────────────────          ────────────────
plaintext .env  ──clean──▶  encrypted blob  (what GitHub sees)
plaintext .env  ◀─smudge──  encrypted blob  (restored on checkout)
```

- **clean** — runs on `git add`; pipes the file through `openssl enc -aes-256-cbc -pbkdf2 -salt` before writing to the index.
- **smudge** — runs on `git checkout`; pipes the stored blob through `openssl dec -aes-256-cbc -pbkdf2` to restore plaintext in the working tree.

The filter is wired up in `.git/config`:

```ini
[filter "git-crypt"]
    clean    = git-crypt clean
    smudge   = git-crypt smudge
    required = true
```

And applied to all `*.env` files via `.gitattributes`:

```
*.env filter=git-crypt diff=git-crypt
```

The encryption key is resolved in priority order:

1. `GIT_CRYPT_KEY` environment variable
2. `pass show git/crypt-key` (GPG-encrypted password store)

### Secret Management Prerequisites

- [`pass`](https://www.passwordstore.org/) with a GPG key configured, **or** the `GIT_CRYPT_KEY` environment variable set
- `openssl` (ships with most Linux distros and macOS)
- The `git-crypt` script installed to your `$PATH`

### Initialising a New Machine

1. Install the `git-crypt` script:

```sh
# Place the script somewhere on your PATH, e.g.
cp git-crypt ~/.local/bin/git-crypt
chmod +x ~/.local/bin/git-crypt
```

2. Make the encryption key available:

```sh
# Option A — password store (recommended)
pass insert git/crypt-key   # paste the shared key when prompted

# Option B — environment variable
export GIT_CRYPT_KEY='<shared-key>'
```

3. Initialise the git filters in the cloned repo:

```sh
git-crypt init
```

This configures the clean/smudge filters in `.git/config`. Existing `.env` files will be decrypted automatically on the next `git checkout` or by running:

```sh
git checkout -- .
```

### Adding a New Service .env

1. Create the `.env` file in the service directory.
2. Force-add it (`.env` is in `.gitignore` as a fallback safety net):

```sh
git add -f <service>/.env
git commit -m "feat(<service>): Add encrypted .env"
```

The clean filter encrypts the file before it ever touches the index. Verify the stored blob is ciphertext with:

```sh
git show HEAD:<service>/.env | xxd | head -3
# Should start with: 53 61 6c 74 65 64 5f 5f  (ASCII "Salted__")
```

## Available Services

* [Audiobookshelf](https://github.com/andronics/homelab-docker-compose/blob/main/audiobookshelf/compose.yml): Audiobook & Podcast Server
* [Authentik](https://github.com/andronics/homelab-docker-compose/blob/main/authentik/compose.yml): Identity Provider & SSO Solution
* [Baserow](https://github.com/andronics/homelab-docker-compose/blob/main/baserow/compose.yml): No-Code Database Platform
* [Bazarr](https://github.com/andronics/homelab-docker-compose/blob/main/bazarr/compose.yml): Subtitle Management for Sonarr & Radarr
* [Booklore](https://github.com/andronics/homelab-docker-compose/blob/main/booklore/compose.yml): Book Library Management
* [Calibre](https://github.com/andronics/homelab-docker-compose/blob/main/calibre/compose.yml): Powerful eBook Manager
* [Checkmk](https://github.com/andronics/homelab-docker-compose/blob/main/checkmk/compose.yml): Infrastructure Monitoring
* [Code](https://github.com/andronics/homelab-docker-compose/blob/main/code/compose.yml): VS Code In The Browser
* [Cross-seed](https://github.com/andronics/homelab-docker-compose/blob/main/cross-seed/compose.yml): Torrent Cross-Seeding Automation
* [Crowdsec](https://github.com/andronics/homelab-docker-compose/blob/main/crowdsec/compose.yml): Curated Threat Intelligence Powered By The Crowd
* [Emby](https://github.com/andronics/homelab-docker-compose/blob/main/emby/compose.yml): Media Server That Streams Audio & Video To Various Devices
* [Error-pages](https://github.com/andronics/homelab-docker-compose/blob/main/error-pages/compose.yml): Custom Error Pages For Traefik
* [Flaresolverr](https://github.com/andronics/homelab-docker-compose/blob/main/flaresolverr/compose.yml): Proxy Server To Bypass Cloudflare Protection
* [Homer](https://github.com/andronics/homelab-docker-compose/blob/main/homer/compose.yml): Full Static HTML/JS Dashboard
* [Immich](https://github.com/andronics/homelab-docker-compose/blob/main/immich/compose.yml): Photo & Video Management Solution
* [Kavita](https://github.com/andronics/homelab-docker-compose/blob/main/kavita/compose.yml): Multi Format Digital Library
* [Lidarr](https://github.com/andronics/homelab-docker-compose/blob/main/lidarr/compose.yml): Music Collection Manager For Usenet And BitTorrent
* [Metube](https://github.com/andronics/homelab-docker-compose/blob/main/metube/compose.yml): YouTube-DL Web Interface
* [MQTT](https://github.com/andronics/homelab-docker-compose/blob/main/mqtt/compose.yml): Message Broker Service
* [N8N](https://github.com/andronics/homelab-docker-compose/blob/main/n8n/compose.yml): Workflow Automation Platform
* [Navidrome](https://github.com/andronics/homelab-docker-compose/blob/main/navidrome/compose.yml): Your Music Collection From Anywhere
* [NordVPN](https://github.com/andronics/homelab-docker-compose/blob/main/nordvpn/compose.yml): VPN Container Network
* [Nuclio](https://github.com/andronics/homelab-docker-compose/blob/main/nuclio/compose.yml): Serverless Functions Platform
* [OpenWebUI](https://github.com/andronics/homelab-docker-compose/blob/main/openwebui/compose.yml): Web UI For LLM Interactions
* [Portainer](https://github.com/andronics/homelab-docker-compose/blob/main/portainer/compose.yml): Container Management
* [Prowlarr](https://github.com/andronics/homelab-docker-compose/blob/main/prowlarr/compose.yml): Indexer Manager & Proxy For *ARR PVR Apps
* [qBittorrent](https://github.com/andronics/homelab-docker-compose/blob/main/qbittorrent/compose.yml): BitTorrent Client
* [Radarr](https://github.com/andronics/homelab-docker-compose/blob/main/radarr/compose.yml): Movie Collection Manager
* [Readarr](https://github.com/andronics/homelab-docker-compose/blob/main/readarr/compose.yml): Book Collection Manager
* [Recyclarr](https://github.com/andronics/homelab-docker-compose/blob/main/recyclarr/compose.yml): TRaSH Guides Sync For Sonarr & Radarr
* [ROMM](https://github.com/andronics/homelab-docker-compose/blob/main/romm/compose.yml): ROM Library Manager
* [Searxng](https://github.com/andronics/homelab-docker-compose/blob/main/searxng/compose.yml): Privacy-Respecting Metasearch Engine
* [Seedbox](https://github.com/andronics/homelab-docker-compose/blob/main/seedbox/compose.yml): Seedbox Management
* [Sonarr](https://github.com/andronics/homelab-docker-compose/blob/main/sonarr/compose.yml): Television Collection Manager
* [Tdarr](https://github.com/andronics/homelab-docker-compose/blob/main/tdarr/compose.yml): Distributed Transcoding System
* [Termix](https://github.com/andronics/homelab-docker-compose/blob/main/termix/compose.yml): Terminal Service
* [Tor](https://github.com/andronics/homelab-docker-compose/blob/main/tor/compose.yml): Overlay Network Enabling Anonymous Communication
* [Traefik](https://github.com/andronics/homelab-docker-compose/blob/main/traefik/compose.yml): Cloud Native Application Proxy
* [Traktarr](https://github.com/andronics/homelab-docker-compose/blob/main/traktarr/compose.yml): Trakt Monitoring Daemon That Adds New TV Series & Movies
* [TVmaze-sync](https://github.com/andronics/homelab-docker-compose/blob/main/tvmaze-sync/compose.yml): TV Show Tracking Sync
* [Unpackerr](https://github.com/andronics/homelab-docker-compose/blob/main/unpackerr/compose.yml): Monitors Downloads And Automatically Extracts Archives
* [Whisparr](https://github.com/andronics/homelab-docker-compose/blob/main/whisparr/compose.yml): Adult Content Management

For detailed information on each service, refer to the respective compose.yml file and any accompanying documentation within the service directory.

## Configuration
Each service directory contains a compose.yml file which can be customized to fit your needs. Common configurations include:

* Ports
* Environment variables
* Volumes

Modify these settings according to your specific requirements before starting the services.

## Contributing

Contributions are welcome! If you have improvements or additional services to add, please fork the repository, create a new branch, and submit a pull request. Ensure your code adheres to the following guidelines:

* Follow the existing directory and file naming conventions.
* Test your changes before submitting.
* Provide a clear description of your changes in the pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](https://github.com/andronics/homelab-docker-compose/blob/main/LICENSE) file for details.

## Contact
For questions, suggestions, or feedback, please open an issue on GitHub or contact me at andronics@gmail.com.
