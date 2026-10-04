# Ansible Login Banner Deployment Playbook

This playbook configures login banners on RHEL 8/9/10 systems to comply with STIG requirements.

It sets:

- CLI login banner (`/etc/issue`) - V-230227, V-257779, V-281227
- SSH banner (`/etc/ssh/banner`) - V-230225, V-257981, V-281224
- Graphical login banner (GNOME/dconf) - V-230226/V-244519, V-270174/V-258012, V-281225/V-281226

A single playbook (`deploy_banner.yml`) applies the correct login banner based on the
`banner_type` variable. Banner texts are defined in `vars/banners.yml`.

| `banner_type` | Description |
| --- | --- |
| `dod` | Department of Defense (DoD) |
| `dcsa` (default) | Defense Counterintelligence and Security Agency (DCSA) |

To add a new banner, add a new entry under the `banners` key in `vars/banners.yml`. See the "Adding a Custom Banner" section below for an example.

## Variables

| Variable | Default | Description |
| --- | --- | --- |
| `banner_type` | `dcsa` | Type of banner to apply (see table above) |

Override at runtime with `-e "banner_type=dod"`

## Usage

### Check mode (dry-run)

```shell
ansible-playbook deploy_banner.yml -c local -i "localhost," --check
```

### Run against local host

```shell
ansible-playbook deploy_banner.yml -c local -i "localhost,"
```

### Run with specific banner type

```shell
ansible-playbook deploy_banner.yml -c local -i "localhost," -e "banner_type=dod"
```

### Run against multiple hosts (inline inventory)

```shell
ansible-playbook deploy_banner.yml -i "computer01, 192.168.1.11," -K -e "banner_type=dod"
```

### Run against multiple hosts (external inventory)

```shell
ansible-playbook -i inventory.ini deploy_banner.yml -K
```

## Adding a Custom Banner

1. Edit `vars/banners.yml`
2. Add a new entry under the `banners` key with `description` and `text` fields
3. Example:

   ```yaml
   custom:
     description: "Custom organization banner"
     text: |
       Welcome to Custom System
       Unauthorized access is prohibited.
   ```
