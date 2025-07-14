
# Top-Level Blocks in Terraform

Terraform configuration is made up of top-level blocks. Each block declares something about your infrastructure or how Terraform itself behaves.

This section explains the most commonly used top-level blocks in Terraform:

---

## 1. `terraform` Block

Used to configure settings related to Terraform itself, like required provider versions or the backend for storing state.

```hcl
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 4.0"
    }
  }

  backend "gcs" {
    bucket = "my-terraform-state"
    prefix = "dev/state"
  }
}
```

---

## 2. `provider` Block

Defines the provider to use (e.g., Google Cloud, AWS). You can configure authentication, region, project, etc.

```hcl
provider "google" {
  project = var.project_id
  region  = var.region
  zone    = var.zone
}
```

You can define multiple providers or even alias providers like this:

```hcl
provider "google" {
  alias   = "us_east"
  region  = "us-east1"
}

provider "google" {
  alias   = "us_west"
  region  = "us-west1"
}
```

---

## 3. `resource` Block

Declares an actual infrastructure resource. Most of your `.tf` files will contain resource blocks.

```hcl
resource "google_compute_instance" "vm_instance" {
  name         = "my-vm"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }

  network_interface {
    network = "default"
    access_config {}
  }
}
```

---

## 4. `variable` Block

Used to define input values that can be customized per environment or user.

```hcl
variable "region" {
  description = "GCP region"
  type        = string
  default     = "us-central1"
}
```

---

## 5. `output` Block

Used to expose values after Terraform applies. Useful for passing info like VM IPs or bucket URLs.

```hcl
output "instance_name" {
  value = google_compute_instance.vm_instance.name
}
```

---

## 6. `module` Block

Used to call a reusable module (a collection of `.tf` configs bundled together).

```hcl
module "vpc" {
  source       = "../modules/vpc"
  project_id   = var.project_id
  region       = var.region
}
```

---

## 7. `data` Block

Used to fetch read-only external data from other resources or APIs (e.g., get an existing image or subnet).

```hcl
data "google_compute_image" "debian" {
  family  = "debian-11"
  project = "debian-cloud"
}
```

---

## 8. `locals` Block

Defines temporary variables for reuse within the config.

```hcl
locals {
  vm_name = "${var.environment}-vm"
}
```

---

## 9. `workspace` (implicitly used)

Terraform uses workspaces to manage different environments like `dev`, `staging`, and `prod`. While not written as a block in the file, you can manage them using CLI:

```bash
terraform workspace new dev
terraform workspace select dev
```

---

## Summary

| Block     | Purpose                                      |
|-----------|----------------------------------------------|
| terraform | Configure Terraform behavior (backend, etc.) |
| provider  | Configure the cloud/API provider             |
| resource  | Define infrastructure components             |
| variable  | Accept input from user or config             |
| output    | Print outputs for use or reference           |
| module    | Reuse existing Terraform configs             |
| data      | Read data from external resources            |
| locals    | Define internal reusable values              |

---

## Reference

- https://developer.hashicorp.com/terraform/language
