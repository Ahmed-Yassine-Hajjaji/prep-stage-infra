# learn-terraform

My first hands-on Terraform project, following the official
[Terraform "Build Infrastructure" tutorial](https://developer.hashicorp.com/terraform/tutorials/docker-get-started).
Instead of a cloud provider, this uses the **Docker provider** to provision an
**Nginx container** locally — a simple way to learn the core Terraform workflow
and basic commands.

## What it does

Terraform pulls the `nginx:latest` image and runs it as a Docker container,
mapping container port `80` to host port `8080`.

| Resource | Description |
|----------|-------------|
| `docker_image.nginx` | Pulls the `nginx:latest` image |
| `docker_container.nginx` | Runs the container, exposing `80 → 8080` |

## Files

| File | Purpose |
|------|---------|
| `main.tf` | Provider config + the image and container resources |
| `variables.tf` | Input variable `container_name` (default: `ExampleNginxContainer`) |
| `outputs.tf` | Outputs the container ID and image ID after apply |
| `.terraform.lock.hcl` | Locks the provider version for reproducible runs |

## Requirements

- [Terraform](https://developer.hashicorp.com/terraform/install) (>= 1.0)
- [Docker](https://docs.docker.com/get-docker/) running locally
- Provider: `kreuzwerker/docker` `~> 4.2.0` (installed automatically by `terraform init`)

## Usage

```bash
# 1. Initialize the working directory and download the Docker provider
terraform init

# 2. Preview the changes Terraform will make
terraform plan

# 3. Create the image and container
terraform apply

# 4. (Optional) override the container name
terraform apply -var "container_name=MyNginx"
```

Once applied, Nginx is reachable at <http://localhost:8080>.

```bash
# Tear everything down when you're done
terraform destroy
```

## Terraform commands I practiced

| Command | What it does |
|---------|--------------|
| `terraform init` | Initializes the directory and installs providers |
| `terraform fmt` | Formats the `.tf` files to canonical style |
| `terraform validate` | Checks the configuration for errors |
| `terraform plan` | Shows what will change before applying |
| `terraform apply` | Provisions / updates the infrastructure |
| `terraform show` | Inspects the current state |
| `terraform output` | Prints the defined outputs |
| `terraform destroy` | Removes all managed resources |

## Notes

- `terraform.tfstate` and the `.terraform/` directory are **gitignored** — state
  can contain sensitive data and provider binaries don't belong in version control.
- Part of the [`prep-stage-infra`](../) monorepo, where each tool I learn lives
  in its own subfolder.
