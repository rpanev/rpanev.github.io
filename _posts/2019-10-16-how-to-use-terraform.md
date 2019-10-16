---
id: 44
title: Как да използваме terraform за aws
date: 2019-10-16T12:48:43+00:00
author: Panev
layout: post
categories:
  - devops
  - linux
tags:
  - devops
---

# Инсталация от source 

1) Ъпдейт на репозиторито

{% highlight shell %}yum update -y
{% endhighlight %}

2) Отваряме страницата на проекта <a href="https://www.terraform.io/" rel="noopener noreferrer" target="_blank">Terraform</a> и сваляме последната катуална версия 
{% highlight shell %}
https://releases.hashicorp.com/terraform/0.12.10/terraform_0.12.10_linux_amd64.zip
{% endhighlight %}

3) Разахивираме приложението

{% highlight shell %}
unzip ./terraform_0.12.10_linux_amd64.zip -d /usr/local/bin/
{% endhighlight %}

4) Един бърз тест:
{% highlight shell %}
$ terraform -v
Terraform v0.12.10
{% endhighlight %}

5) Създаваме си файл aws-vps.tf

{% highlight shell %}
touch aws-vps.tf
{% endhighlight %}

6) Попълваме нужната информация за вдигане на VPS, без много екстри
{% highlight terraform %}
provider "aws" {
  region     = "eu-central-1"
  access_key = "accesskey"
  secret_key = "secretkey"
}

resource "aws_instance" "panev_test_terraform" {
  ami = "ami-9a183671"
  instance_type = "t2.micro"
}
{% endhighlight %}

7) След това трябва да инициализираме Terraform:

{% highlight shell %}
$ terraform init

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "aws" (hashicorp/aws) 2.32.0...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.aws: version = "~> 2.32"


Warning: Skipping backend initialization pending configuration upgrade

The root module configuration contains errors that may be fixed by running the
configuration upgrade tool, so Terraform is skipping backend initialization.
See below for more information.


Terraform has initialized, but configuration upgrades may be needed.

Terraform found syntax errors in the configuration that prevented full
initialization. If you've recently upgraded to Terraform v0.12, this may be
because your configuration uses syntax constructs that are no longer valid,
and so must be updated before full initialization is possible.

Terraform has installed the required providers to support the configuration
upgrade process. To begin upgrading your configuration, run the following:
    terraform 0.12upgrade

To see the full set of errors that led to this message, run:
    terraform validate
{% endhighlight %}

8) Валидираме нашият код
{% highlight shell %}
terraform validate
{% endhighlight %}
 
- При грешка ще ни каже къде е проблема :

{% highlight shell %}
$ terraform validate

Error: Unsupported block type

  on vps.tf line 1:
   1: cat aws-vps.tf

Blocks of type "cat" are not expected here. Did you mean "data"?


Error: Invalid block definition

  on aws-vps.tf line 1:
   1: cat aws-vps.tf

Either a quoted string block label or an opening brace ("{") is expected here.

{% endhighlight %}

- Когато всичко е както трябва :

{% highlight shell %}
$ terraform validate
Success! The configuration is valid.
{% endhighlight %}

9) Може да изпозлваме комнадата `terraform plan` и така, ще направим симулация без да направим нищо в aws:
{% highlight shell %}
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.panev_test_terraform will be created
  + resource "aws_instance" "panev_test_terraform" {
      + ami                          = "ami-9a183671"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
      + availability_zone            = (known after apply)
      + cpu_core_count               = (known after apply)
      + cpu_threads_per_core         = (known after apply)
      + get_password_data            = false
      + host_id                      = (known after apply)
      + id                           = (known after apply)
      + instance_state               = (known after apply)
      + instance_type                = "t2.micro"
      + ipv6_address_count           = (known after apply)
      + ipv6_addresses               = (known after apply)
      + key_name                     = (known after apply)
      + network_interface_id         = (known after apply)
      + password_data                = (known after apply)
      + placement_group              = (known after apply)
      + primary_network_interface_id = (known after apply)
      + private_dns                  = (known after apply)
      + private_ip                   = (known after apply)
      + public_dns                   = (known after apply)
      + public_ip                    = (known after apply)
      + security_groups              = (known after apply)
      + source_dest_check            = true
      + subnet_id                    = (known after apply)
      + tenancy                      = (known after apply)
      + volume_tags                  = (known after apply)
      + vpc_security_group_ids       = (known after apply)

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.

{% endhighlight %}

10) След като се убедим в това което искаме изпълняваме `terraform apply`  и потвърждаме на въпроса с *yes* 
{% highlight shell %}
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.panev_test_terraform will be created
  + resource "aws_instance" "panev_test_terraform" {
      + ami                          = "ami-9a183671"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
      + availability_zone            = (known after apply)
      + cpu_core_count               = (known after apply)
      + cpu_threads_per_core         = (known after apply)
      + get_password_data            = false
      + host_id                      = (known after apply)
      + id                           = (known after apply)
      + instance_state               = (known after apply)
      + instance_type                = "t2.micro"
      + ipv6_address_count           = (known after apply)
      + ipv6_addresses               = (known after apply)
      + key_name                     = (known after apply)
      + network_interface_id         = (known after apply)
      + password_data                = (known after apply)
      + placement_group              = (known after apply)
      + primary_network_interface_id = (known after apply)
      + private_dns                  = (known after apply)
      + private_ip                   = (known after apply)
      + public_dns                   = (known after apply)
      + public_ip                    = (known after apply)
      + security_groups              = (known after apply)
      + source_dest_check            = true
      + subnet_id                    = (known after apply)
      + tenancy                      = (known after apply)
      + volume_tags                  = (known after apply)
      + vpc_security_group_ids       = (known after apply)

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.panev_test_terraform: Creating...
aws_instance.panev_test_terraform: Still creating... [10s elapsed]
aws_instance.panev_test_terraform: Still creating... [20s elapsed]
aws_instance.panev_test_terraform: Still creating... [30s elapsed]
aws_instance.panev_test_terraform: Creation complete after 34s [id=i-0aae21339de0060c2]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
{% endhighlight %}

11) За да премахнем инстанцията пишем `terraform destroy` и потвърждаме на въпроса с *yes* 
{% highlight shell %}
$ terraform destroy
aws_instance.panev_test_terraform: Refreshing state... [id=i-084ff7c42ceb73240]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # aws_instance.panev_test_terraform will be destroyed
  - resource "aws_instance" "panev_test_terraform" {
      - ami                          = "ami-9a183671" -> null
      - arn                          = "arn:aws:ec2:eu-central-1:918488250450:instance/i-084ff7c42ceb73240" -> null
      - associate_public_ip_address  = true -> null
      - availability_zone            = "eu-central-1b" -> null
      - cpu_core_count               = 1 -> null
      - cpu_threads_per_core         = 1 -> null
      - disable_api_termination      = false -> null
      - ebs_optimized                = false -> null
      - get_password_data            = false -> null
      - id                           = "i-084ff7c42ceb73240" -> null
      - instance_state               = "running" -> null
      - instance_type                = "t2.micro" -> null
      - ipv6_address_count           = 0 -> null
      - ipv6_addresses               = [] -> null
      - monitoring                   = false -> null
      - primary_network_interface_id = "eni-0ae9762298a6f00bc" -> null
      - private_dns                  = "ip-172-31-36-173.eu-central-1.compute.internal" -> null
      - private_ip                   = "172.31.36.173" -> null
      - public_dns                   = "ec2-18-185-69-50.eu-central-1.compute.amazonaws.com" -> null
      - public_ip                    = "18.185.69.50" -> null
      - security_groups              = [
          - "default",
        ] -> null
      - source_dest_check            = true -> null
      - subnet_id                    = "subnet-7a391307" -> null
      - tags                         = {} -> null
      - tenancy                      = "default" -> null
      - volume_tags                  = {} -> null
      - vpc_security_group_ids       = [
          - "sg-57d0ac36",
        ] -> null

      - credit_specification {
          - cpu_credits = "standard" -> null
        }

      - root_block_device {
          - delete_on_termination = false -> null
          - encrypted             = false -> null
          - iops                  = 100 -> null
          - volume_id             = "vol-0ac02fec6b74e9030" -> null
          - volume_size           = 8 -> null
          - volume_type           = "gp2" -> null
        }
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

aws_instance.panev_test: Destroying... [id=i-084ff7c42ceb73240]
aws_instance.panev_test: Still destroying... [id=i-084ff7c42ceb73240, 10s elapsed]
aws_instance.panev_test: Still destroying... [id=i-084ff7c42ceb73240, 20s elapsed]
aws_instance.panev_test: Still destroying... [id=i-084ff7c42ceb73240, 30s elapsed]
aws_instance.panev_test: Destruction complete after 30s

Destroy complete! Resources: 1 destroyed.

{% endhighlight %}