
# 📝 Terraform Syntax

This section introduces Terraform’s configuration language — **HCL (HashiCorp Configuration Language)** — which is used to define infrastructure as code.

---

## 📦 Structure of Terraform Syntax

Terraform syntax is structured using blocks:
    
```hcl
<block_type> "<label1>" "<label2>" {
  argument_name = value
}
```

Example:

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

---

## 🔹 Common Block Types

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

## 🧠 Data Types

| Type    | Example                          |
|---------|----------------------------------|
| String  | `"us-west-1"`                    |
| Number  | `5`, `3.14`                      |
| Boolean | `true`, `false`                  |
| List    | `["dev", "staging", "prod"]`     |
| Map     | `{ region = "us-west-1" }`       |
| Object  | `{ name = "app", port = 8080 }`  |

---

## 🪄 Expressions and Interpolation

- Access variables: `var.name`
- Access resource attributes: `aws_instance.web.id`
- Interpolation (old style): `"${var.name}-bucket"`

Since Terraform 0.12+, direct references like `var.name` are preferred over `"${}"`.

---

## 🛠 Example Configuration

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

provider "aws" {
  region = var.region
}

resource "aws_s3_bucket" "example" {
  bucket = "${var.environment}-my-bucket"
  acl    = "private"
}
```

---

## 📌 Best Practices

- Use indentation and spacing consistently
- Keep blocks short and readable
- Use `locals` for reusability
- Avoid hardcoding values directly in resources

---

## 📖 References

- [Terraform Language Documentation](https://developer.hashicorp.com/terraform/language)
