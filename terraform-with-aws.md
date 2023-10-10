# Terraform

## Basic Principles

> The **order** in which resources are **declared** in your *configuration file*s has **no effect** on the order in which *Terraform* creates or destroys them.

> *Terraform* will wait to create the *dependent* *resource* until after the specified *resource* (**dependency**) is created

use `depends_on` argument to declare *explicit* dependencies

## Glossary

+ **ECS** = *Elastic Container Service*
+ **ACL** = 
+ **EBS** = *Elastic Block Store*
+ **AMI**s = *Amazon Machine Images*

+ **local values** (or "**locals**") **assign** a *name* to an *expression* or *value*

> All of your configuration's local values can be defined in a single locals block, or you can use multiple blocks.
> Unlike variables found in programming languages, Terraform's locals do not change values during or between Terraform runs such as plan, apply, or destroy. You can use locals to give a name to the result of any Terraform expression, and re-use that name throughout your configuration. Unlike input variables, locals are not set directly by users of your configuration.
> Unlike variable values, local values can use dynamic expressions and resource arguments.

### Resources

Resources have arguments, attributes, and meta-arguments.

> Arguments configure a particular resource; because of this, many arguments are resource-specific. Arguments can be required or optional, as specified by the provider. If you do not supply a required argument, Terraform will give an error and not apply the configuration.
> Attributes are values exposed by an existing resource. References to resource attributes take the format resource_type.resource_name.attribute_name. Unlike arguments which specify an infrastructure object's configuration, a resource's attributes are often assigned to it by the underlying cloud provider or API.
> Meta-arguments change a resource's behavior, such as using a count meta-argument to create multiple resources. Meta-arguments are a function of Terraform itself and are not resource or provider-specific.

+ *resource* **type** 
+ *resource* **name**
+ *resource* **identifier** (**ID**)
> Resource *blocks* declare a resource *type* and *name*. Together, the *type* and *name* form a resource *identifier (ID)* in the format `resource_type.resource_name`, in this case random_pet.name. The resource's *ID* must be **unique** within a *workspace*


##### Workflows
+ A **VCS**-driven *workflow*, in which it automatically queues plans whenever changes are **committed** to your configuration's *VCS* repo.
+ An **API**-driven *workflow*, in which a *CI pipeline* or other **automated tool** can upload configurations directly.

---
## Notes
*AMI ID*s are **region-specific**

> Setting variables via the command-line will not save their values. Terraform supports many ways to use and set variables so you can avoid having to enter them repeatedly as you execute commands.

### Snippets

##### random Names

`resource "random_pet" "name" {}`

> The resource block defines a random_pet resource named name, which generates a random pet name`

##### endpoint and URL outputs

+ EC2 instance's *domain* 
```json
output "domain-name" {
  value = aws_instance.web.public_dns
}
```
+ EC2 instance's *endpoint* 
```json
output "application-url" {
  value = "${aws_instance.web.public_dns}/index.php"
}
```

Examples

##### Locals and variables

`main.tf` file:
```json
locals {
  name_suffix = "${var.project_name}-${var.environment}"
}

locals {
  required_tags = {
    project     = var.project_name,
    environment = var.environment
  }
  tags = merge(var.resource_tags, local.required_tags)
}
```
`variables.tf` file:
```properties

variable "environment" {
  description = "Name of the environment."
  type        = string
  default     = "dev"
}
```

bash: `terraform apply -var "environment=prod"`