
# Terraform Syntax

This section introduces Terraform’s configuration language — HCL (HashiCorp Configuration Language) — which is used to define infrastructure as code.

---

## Structure of Terraform Syntax

Terraform syntax is structured using blocks:

```hcl
<block_type> "<label1>" "<label2>" {
  argument_name = value
}
```

Example:

```hcl
resource "google_compute_instance" "example" {
  name         = "vm-instance"
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

## Common Block Types

| Block        | Purpose                                  |
|--------------|-------------------------------------------|
| `terraform`  | Configure Terraform behavior              |
| `provider`   | Define which cloud provider to use        |
| `resource`   | Declare cloud infrastructure resources    |
| `variable`   | Define inputs                             |
| `output`     | Define outputs                            |
| `module`     | Reuse other configurations                |
| `data`       | Access read-only external information     |
| `locals`     | Store reusable local values               |

---

## Data Types

| Type    | Example                          |
|---------|----------------------------------|
| String  | `"us-central1"`                  |
| Number  | `5`, `3.14`                      |
| Boolean | `true`, `false`                  |
| List    | `["dev", "staging", "prod"]`     |
| Map     | `{ region = "us-central1" }`     |
| Object  | `{ name = "vm", cpu = 2 }`       |

---

## Expressions and Interpolation

- Access variables: `var.project_id`
- Access resource attributes: `google_compute_instance.example.name`
- Interpolation (older style): `"${var.project_id}-bucket"`

Direct references like `var.project_id` are preferred since Terraform 0.12+.

---

## Example GCP Configuration

```hcl
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 4.0"
    }
  }
}

provider "google" {
  project = var.project_id
  region  = var.region
  zone    = var.zone
}

resource "google_compute_instance" "example" {
  name         = "vm-instance"
  machine_type = "e2-medium"
  zone         = var.zone

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

## Best Practices

- Use indentation and spacing consistently
- Keep blocks short and readable
- Use `locals` for reusability
- Avoid hardcoding values directly in resources
- Store sensitive values using `terraform.tfvars` or environment variables

---

## References

- [Terraform Language Documentation](https://developer.hashicorp.com/terraform/language)
- [Terraform GCP Provider Docs](https://registry.terraform.io/providers/hashicorp/google/latest/docs)
