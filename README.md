# Nginx service for Wodby

Use Nginx as a reusable component in applications managed by Wodby. This
repository contains the service manifests and referenced files used by the
public Nginx service in the Wodby catalog.

- [Nginx service in the Wodby catalog](https://wodby.com/services/nginx)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `nginx` |
| Type | Application service |
| Versions | `1.31` by default; also available: `1.30`, `1.29`, `1.28`, `1.27`, `1.25` |
| Workloads | `main` (Deployment), primary; scalable |
| Containers | `nginx` using `wodby/nginx`, build target |
| Endpoints | `http`: HTTP 80 (main) |
| Service links | Backend, optional |
| Application build | Git source connection enabled; starters: React boilerplate |
| Helm | chart `oci://registry-1.docker.io/wodby/nginx`; version `0.2.1` |
| Configuration | 1 settings, 2 configuration files |

## Use this service

A service is a reusable component and does not deploy by itself. Add the public
catalog service to a stack, configure its required links and settings, publish
the stack, and then create or upgrade an app instance.

To maintain your own version of this service:

1. Fork this repository.
2. Edit the service manifest and any files it references.
3. Import the repository as a
   [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference `nginx` from your stack manifest.

Wodby imports the manifest and referenced files from the selected Git branch or
tag and creates a new service revision when the Git-backed service is updated.

## Customize the service

Common changes include adjusting versions, images, Helm chart settings, build
inputs, environment variables, links, storage, resources, and operational
workflows supported by the manifest.

Keep service, workload, container, endpoint, link, volume, config, and
derivative names stable unless dependent stacks and app-level overrides are
updated at the same time. These names are part of the contract consumed by
downstream manifests.

Validate customized manifests with the Wodby CLI before importing them:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/)
for every supported field and the [managed services
index](https://github.com/wodby/services) for more service examples.
