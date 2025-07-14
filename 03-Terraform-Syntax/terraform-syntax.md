  
# Terraform Syntax Explained

Terraform uses HCL (HashiCorp Configuration Language) to describe infrastructure in a readable and structured format. This file explains the core building blocks of Terraform syntax.

---

## What Is Terraform Syntax?

Terraform configurations are written in HCL (HashiCorp Configuration Language), which is designed to be both human-readable and machine-friendly. It uses a block-based structure with arguments and expressions to define infrastructure resources.

---

## Basic Syntax Structure

Terraform uses blocks to define each infrastructure component:

```hcl
<block_type> "<label_1>" "<label_2>" {
  argument_name = value
}
```

### Example:

```hcl
resource "google_compute_instance" "vm_instance" {
  name         = "my-instance"
  machine_type = "e2-medium"
  zone         = "us-central1-a"
}
```

Explanation:
- `resource`: Block type
- `"google_compute_instance"`: Resource type (GCP VM)
- `"vm_instance"`: Resource name
- Inside `{}`: Arguments

---

## Components of Terraform Syntax

### 1. Blocks

Blocks are the top-level elements of Terraform syntax. Each block starts with a type (like `resource`, `provider`, `variable`, etc.) and may include one or more labels.

Examples:

```hcl
provider "google" {
  project = var.project_id
  region  = var.region
}

resource "google_storage_bucket" "example" {
  name     = "my-storage-bucket"
  location = "US"
}
```

---

### 2. Arguments

Arguments are key-value pairs inside blocks. They configure the behavior or attributes of the block.

Example:

```hcl
machine_type = "e2-medium"
zone         = "us-central1-a"
```

---

### 3. Expressions

Expressions compute or reference values. Terraform supports string literals, numbers, lists, maps, and references to variables or other resources.

Example:

```hcl
bucket = var.project_id
```

Older style interpolation (still valid but less recommended after 0.12):

```hcl
bucket = "${var.project_id}-my-bucket"
```

---

### 4. Labels

Many blocks have one or more labels. For example:

```hcl
resource "google_compute_instance" "example" {
  ...
}
```

- First label: Resource type (`google_compute_instance`)
- Second label: Resource name (`example`)
- Reference in code: `google_compute_instance.example`

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

## Example Configuration (GCP)

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

- Use consistent indentation and formatting
- Split large configurations across multiple files
- Use `variables` and `locals` to avoid hardcoding
- Avoid embedding secrets directly in `.tf` files
- Group related resources in logical files or modules

---

## References

- [Terraform Configuration Language](https://developer.hashicorp.com/terraform/language)
- [GCP Provider Docs](https://registry.terraform.io/providers/hashicorp/google/latest/docs)
