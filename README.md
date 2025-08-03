# beingtomgreen.sonarr_docker

A simple ansible role for running [Sonarr](https://sonarr.tv/) via Docker compose using the [LinuxServer.io Sonarr image](https://docs.linuxserver.io/images/docker-sonarr).

For additional information on how to best handle your media paths see information on [wiki.servarr.com](https://wiki.servarr.com/docker-guide#consistent-and-well-planned-paths) and [trash-guides.info](https://trash-guides.info/File-and-Folder-Structure/Hardlinks-and-Instant-Moves/).

## Installation

Given that Galaxy seems to have abandoned roles, I suggest referencing this repository directly in your projects `requirements.yml`:

```yaml
---

roles:
  - name: beingtomgreen.sonarr_docker
    src: https://github.com/BeingTomGreen/ansible-role-sonarr-docker.git

collections: []
```

You can then install it alongside your other requirements as normal:

```bash
ansible-galaxy install -r requirements.yml
```

## Example usage

### Basic usage

```yaml
---

- hosts: sonarr
  become: true
  vars:
    sonarr_docker_env_puid: 5000
    sonarr_docker_env_pgid: 5000

    sonarr_docker_additional_volumes:
        - '/path/to/tv:/tv'
        - '/path/to/usenet:/usenet'
        - '/path/to/torrents:/torrents'
  roles:
    - role: beingtomgreen.sonarr_docker
```

### Want the kitchen sink?

Seriously, take a look at [`defaults/main.yml`](defaults/main.yml), it's obnoxiously commented, just for you.

```yaml
---

- hosts: sonarr
  become: true
  vars:
    sonarr_docker_cap_add:
      - CAP_WAKE_ALARM
      - CAP_AUDIT_CONTROL
    sonarr_docker_cap_drop:
      - CAP_CHECKPOINT_RESTORE

    sonarr_docker_container_name: 'sonarr_container'

    sonarr_docker_depends_on:
      - 'my-little service'

    sonarr_docker_devices:
      - '/dev/dri:/dev/dri'
      - '/dev/kfd:/dev/kfd'

    sonarr_docker_env_puid: 5000
    sonarr_docker_env_pgid: 5000

    sonarr_docker_env_timezone: 'Europe/London'

    sonarr_docker_extra_environment_vars:
      SOME_OTHER_VAR: 'A_VALUE'

    sonarr_docker_image_tag: '10.10.6'

    sonarr_docker_labels:
      traefik.enable: 'true'
      traefik.http.routers.traefik-https.entrypoints: 'https'

    # Network mode
    sonarr_docker_network_mode: 'service:glutun'

    # Or external networks:
    sonarr_docker_external_networks:
      - 'my-little-network'

    # Or the default ports
    sonarr_docker_ports:
      - '8989:8989'

    sonarr_docker_pull_policy: 'weekly'

    sonarr_docker_restart_policy: 'always'

    sonarr_docker_additional_volumes:
      - '/path/to/tv:/tv'
      - '/path/to/usenet:/usenet'
      - '/path/to/torrents:/torrents'

    sonarr_docker_base_directory: '/opt/sonarr_docker'

    sonarr_docker_prune_images: 'true'
    sonarr_docker_prune_until: '24h'

  roles:
    - role: beingtomgreen.sonarr_docker
```

## Role Variables

See [`defaults/main.yml`](defaults/main.yml) for more details.

## Requirements

- Docker & docker compose installed on the target host
- ansible_user will also need to be in the docker group

## Dependencies

- community.docker

## License

[MIT](LICENSE)

## Author Information

[Tom Green](https://github.com/BeingTomGreen)
