# AWV GitHub Action

Run the [auto-website-visitor](https://pypi.org/project/auto-website-visitor) Python package via GitHub Actions with full CLI and config file support — powered by browser automation, scheduling, proxies, and more.

> \[!CAUTION]
> This action is intended for ethical testing, automation, or educational purposes only. Misuse may violate website terms of service or local laws.


## Overview

`awv-action` is a GitHub Action wrapper for the Python package [`auto-website-visitor`](https://pypi.org/project/auto-website-visitor), enabling automated website visits inside GitHub workflows.

Based on [auto-website-visitor](https://github.com/nayandas69/auto-website-visitor)

| Feature              | Status      |
|----------------------|-------------|
| Multi-browser support | ✅          |
| Headless mode         | ✅          |
| Proxy support         | ✅ (with secrets) |
| Config file support   | ✅ (YAML/JSON)    |
| Random delay          | ✅          |
| Interactive CLI       | 🚫 (not used in Actions) |
| Scheduled runs        | ✅ (via GitHub cron) |


## Features

- [x] Use directly in GitHub Actions
- [x] Supports Chrome, Firefox, and Edge
- [x] Headless mode for speed
- [x] Secure proxy support using GitHub Secrets
- [x] Use config files or CLI-style inputs
- [x] Enable auto-scrolling and delays
- [x] Real-time logs and retry logic

> \[!TIP]
> Great for testing UIs, running traffic simulations, or scheduled visits to validate deployments.


## Quick Start

### Minimal Usage

```yaml
jobs:
  visit:
    runs-on: ubuntu-latest
    steps:
      - uses: nayandas69/awv-action@v1
        with:
          url: 'https://example.com'
```


### Full Example

```yaml
jobs:
  visit-website:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code (optional if using config file)
        uses: actions/checkout@v4

      - name: Run Website Visitor
        uses: nayandas69/awv-action@v1
        with:
          url: 'https://example.com'         # Required unless using a config file
          count: '5'                         # Number of visits
          browser: 'chrome'                  # Options: chrome, firefox, edge
          headless: 'true'                   # Headless mode
          proxy: ${{ secrets.MY_PROXY }}     # Proxy via GitHub Secret
          auto_scroll: 'true'                # Simulate scrolls
          random_delay: 'true'               # Random wait times
          interval: '5'                      # Wait 5 seconds between visits
          log_level: 'INFO'                  # Log verbosity
```


### Using a Configuration File

Create a config file (`awv-config.yaml`) in your repo:

```yaml
url: https://example.com
visit_count: 3
browser: firefox
headless: true
interval: 10
auto_scroll: true
proxy: 127.0.0.1:8080
random_delay: true
log_level: DEBUG
```

Then reference it in your workflow:

```yaml
- uses: nayandas69/awv-action@v1
  with:
    config_file: './awv-config.yaml'
```

> \[!NOTE]
> If you specify `config_file`, all other individual inputs are ignored.


## Inputs

| Name           | Description                                          | Required                | Default  |
| -------------- | ---------------------------------------------------- | ----------------------- | -------- |
| `url`          | Website to visit                                     | ✅ (unless using config) | -        |
| `count`        | Number of visits to make                             | ❌                       | `1`      |
| `browser`      | Browser to use: `chrome`, `firefox`, `edge`          | ❌                       | `chrome` |
| `headless`     | Run in headless mode                                 | ❌                       | `true`   |
| `proxy`        | Proxy server (e.g. `ip:port` or `user:pass@ip:port`) | ❌                       | -        |
| `config_file`  | Path to YAML or JSON config file                     | ❌                       | -        |
| `interval`     | Seconds to wait between visits                       | ❌                       | -        |
| `auto_scroll`  | Enable automatic scrolling behavior                  | ❌                       | `false`  |
| `random_delay` | Enable random delay between actions                  | ❌                       | `false`  |
| `log_level`    | Logging level: `DEBUG`, `INFO`, `WARNING`, `ERROR`   | ❌                       | `INFO`   |


## Using GitHub Secrets for Proxy

Store your proxy in GitHub Secrets (e.g. `MY_PROXY`) and use like:

```yaml
proxy: ${{ secrets.MY_PROXY }}
```

> \[!IMPORTANT]
> Never hard-code credentials or IPs in public workflows.


## Example with `workflow_dispatch`

```yaml
# .github/workflows/visit-example.yml

name: Website Visit Example

on:
  workflow_dispatch:

jobs:
  visit:
    runs-on: ubuntu-latest
    steps:
      - uses: nayandas69/awv-action@v1
        with:
          url: 'https://example.com'
          count: '5'
          browser: 'chrome'
          headless: 'true'
```


## Output Logs

Example log output:

```
2025-07-01 14:12:00 - INFO - Visiting: https://example.com
2025-07-01 14:12:03 - INFO - Visit completed
```

> \[!TIP]
> Use `log_level: DEBUG` for troubleshooting or dev use.


## Related Links

* Core Project (PyPI): [auto-website-visitor](https://pypi.org/project/auto-website-visitor)
* Source Code: [auto-website-visitor GitHub](https://github.com/nayandas69/auto-website-visitor)
* GitHub Action Repo: [awv-action](https://github.com/nayandas69/awv-action)


## License

This project is licensed under the MIT License - see the [`LICENSE`](./LICENSE) file for details.

> \[!NOTE]
> Contributions are welcome — feel free to open PRs or suggest improvements.
