# zypper-restart-services-plugin

A Zypper plugin that detects services affected by package updates and assists in restarting them.

This plugin integrates with Zypper’s plugin interface and is intended to improve system consistency
after package upgrades by identifying services that should be restarted.

---

## Overview

When packages are updated, running services may continue using outdated binaries or libraries.
This plugin helps by:

- Detecting services affected by package updates
- Providing restart recommendations
- Maintaining a log file (`/var/log/zypp/restart-services-plugin.log`) with all detected services and actions
- Integrating seamlessly with the Zypper workflow

The plugin is primarily intended for openSUSE systems and is packaged and built using the **openSUSE Build Service (OBS)**.

---

## Usage

The plugin is invoked automatically by **zypper** when installed.

Typical usage example:

```bash
$ sudo zypper update

After the transaction, the plugin evaluates which services may need restarting and reports them accordingly.
All actions and detected services are recorded in:

```bash
/var/log/zypper-restart-services.log

## Installation
### openSUSE

Install the package from the appropriate repository:

$ sudo zypper install zypper-restart-services-plugin


If the package is not yet available in your distribution, you may install it from OBS:

$ sudo zypper ar -f https://download.opensuse.org/repositories/home:/rhabacker:/maintenance/<distribution>/ maintenance
$ sudo zypper install zypper-restart-services-plugin


Replace <distribution> with your openSUSE version.

## Packaging and Development
### Source of Truth

This package is currently maintained directly in the openSUSE Build Service:

OBS package:
https://build.opensuse.org/package/show/home:rhabacker:maintenance/zypper-restart-services-plugin

All packaging files (spec file, sources, patches) are managed there.

## Development Workflow

- Changes are made using osc or the OBS web interface
- OBS automatically builds the package for configured targets
- The OBS history represents the authoritative change history

At the moment, there is no separate upstream Git repository.

## Contributing

Contributions are welcome.

### Preferred contribution methods

Submit patches via OBS (osc checkout, osc commit)

Open a bug or enhancement request via openSUSE Bugzilla

If a Git-based workflow (e.g. GitHub + OBS integration) is introduced in the future, contribution instructions will be updated accordingly.

## License

This project is licensed under the terms specified in the RPM spec file.

Please refer to the package sources in OBS for full licensing information.

## Maintainer

Ralf Habacker
