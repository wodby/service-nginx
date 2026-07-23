# Nginx service for Kubernetes on Wodby

Build and run Nginx applications on Kubernetes with Wodby.

This repository defines the Wodby service manifests and operational
configuration for Nginx.

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Start with a template

Use one of the source templates exposed by this service to start with
compatible build configuration and Wodby CI:

- [React boilerplate](https://github.com/wodby/react-boilerplate)

## Wodby stacks using this service

- [HTML application stack](https://github.com/wodby/stack-html)

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

Use this service through [HTML application stack](https://github.com/wodby/stack-html), or reference `nginx` from a custom
Wodby stack.

A service is a reusable component and does not deploy by itself. The stack
defines its links, settings, versions, resources, and relationship to the rest
of the application.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Keep service, workload, container, endpoint, link, volume, config, and
derivative names stable unless dependent stacks and app-level overrides are
updated at the same time.

Validate the manifests with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
