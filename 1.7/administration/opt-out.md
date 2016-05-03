---
post_title: Opt-Out
menu_order: 6
---

## Authentication

You can opt-out of the provided authentication by disabling it for your cluster. To disable authentication, add this parameter to your [`config.yaml`][4] file during installation (note this requires using the [CLI][1] or [advanced][2] installers):

```yaml
oauth_enabled: 'false'
```

Note that if you’ve already installed your cluster and would like to disable this in-place, you can go through an upgrade with the same parameter set.

## Telemetry

You can opt-out of providing anonymous data by disabling telemetry for your cluster. To disable telemetry, add this parameter to your [`config.yaml`][4] file during installation (note this requires using the [CLI][1] or [advanced][2] installers):

```yaml
telemetry_enabled: 'false'
```

Note that if you’ve already installed your cluster and would like to disable this in-place, you can go through an [upgrade][3] with the same parameter set.

[1]: ../installing/custom/cli/
[2]: ../installing/custom/advanced/
[3]: ../installing/custom/configuration-parameters/