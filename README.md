<img width="559" height="292" alt="Screenshot 2026-03-01 at 9 44 18 AM" src="https://github.com/user-attachments/assets/3d67060b-0660-4377-97b1-b7e36d196fe5" />

# 1. Add import block for ec2 instance and add instance id

$ vi main.tf
```
provider "aws" {
    region = "ap-south-2"
}

import {
    id = "i-0bc5dc8c2bdcf7d78"
    to = aws_instance.webserver
}
```

# 2. Execute terraform plan and extract the config

$  terraform plan -generate-config-out=exisitng_resources.tf
```
aws_instance.webserver: Preparing import... [id=i-0bc5dc8c2bdcf7d78]
aws_instance.webserver: Refreshing state... [id=i-0bc5dc8c2bdcf7d78]
```

# 3. Copy paste the generated config for ec2 instance, delete the .tf file and remove import block

$ vi main.tf
```
provider "aws" {
    region = "ap-south-2"
}

resource "aws_instance" "webserver" {
  ami                                  = "ami-00e42015cc6980619"
  associate_public_ip_address          = true
  availability_zone                    = "ap-south-2a"
  disable_api_stop                     = false
  disable_api_termination              = false
  ebs_optimized                        = true
  force_destroy                        = false
  get_password_data                    = false
  hibernation                          = false
  instance_initiated_shutdown_behavior = "stop"
  instance_type                        = "t3.micro"
  monitoring                           = false
  placement_partition_number           = 0
  private_ip                           = "172.31.0.253"
  region                               = "ap-south-2"
  secondary_private_ips                = []
  security_groups                      = ["launch-wizard-1"]
  source_dest_check                    = true
  subnet_id                            = "subnet-0cfa1aa04c041aee0"
  tags = {
    Name        = "python-flask-app-webserver"
    Secret      = "Vipins app"
    environment = "dev"
  }
  tags_all = {
    Name        = "python-flask-app-webserver"
    Secret      = "Vipins app"
    environment = "dev"
  }
  tenancy                     = "default"
  user_data                   = null
  user_data_replace_on_change = null
  volume_tags                 = null
  vpc_security_group_ids      = ["sg-0ea16b1a3a05ed2ab"]
  capacity_reservation_specification {
    capacity_reservation_preference = "open"
  }
  cpu_options {
    core_count       = 1
    threads_per_core = 2
  }
  credit_specification {
    cpu_credits = "unlimited"
  }
  enclave_options {
    enabled = false
  }
  maintenance_options {
    auto_recovery = "default"
  }
  metadata_options {
    http_endpoint               = "enabled"
    http_protocol_ipv6          = "disabled"
    http_put_response_hop_limit = 2
    http_tokens                 = "required"
    instance_metadata_tags      = "disabled"
  }
  private_dns_name_options {
    enable_resource_name_dns_a_record    = true
    enable_resource_name_dns_aaaa_record = false
    hostname_type                        = "ip-name"
  }
  root_block_device {
    delete_on_termination = true
    encrypted             = false
    iops                  = 3000
    tags                  = {}
    tags_all              = {}
    throughput            = 125
    volume_size           = 10
    volume_type           = "gp3"
  }
}
```

# 4. Execute terraform plan and see that TF is unable to identify the resource, as it is not yet added to state file

$ terraform plan
```
Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.webserver will be created
  + resource "aws_instance" "webserver" {
      + ami                                  = "ami-00e42015cc6980619"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = true
      + availability_zone                    = "ap-south-2a"
      + disable_api_stop                     = false
      + disable_api_termination              = false
      + ebs_optimized                        = true
      + enable_primary_ipv6                  = (known after apply)
      + force_destroy                        = false
      + get_password_data                    = false
      + hibernation                          = false
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = "stop"
      + instance_lifecycle                   = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t3.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = false
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_group_id                   = (known after apply)
      + placement_partition_number           = 0
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = "172.31.0.253"
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + region                               = "ap-south-2"
      + secondary_private_ips                = (known after apply)
      + security_groups                      = [
          + "launch-wizard-1",
        ]
      + source_dest_check                    = true
      + spot_instance_request_id             = (known after apply)
      + subnet_id                            = "subnet-0cfa1aa04c041aee0"
      + tags                                 = {
          + "Name"        = "python-flask-app-webserver"
          + "Secret"      = "Vipins app"
          + "environment" = "dev"
        }
      + tags_all                             = {
          + "Name"        = "python-flask-app-webserver"
          + "Secret"      = "Vipins app"
          + "environment" = "dev"
        }
      + tenancy                              = "default"
      + user_data_base64                     = (known after apply)
      + user_data_replace_on_change          = false
      + vpc_security_group_ids               = [
          + "sg-0ea16b1a3a05ed2ab",
        ]

      + capacity_reservation_specification {
          + capacity_reservation_preference = "open"
        }

      + cpu_options {
          + amd_sev_snp           = (known after apply)
          + core_count            = 1
          + nested_virtualization = (known after apply)
          + threads_per_core      = 2
        }

      + credit_specification {
          + cpu_credits = "unlimited"
        }

      + ebs_block_device (known after apply)

      + enclave_options {
          + enabled = false
        }

      + ephemeral_block_device (known after apply)

      + instance_market_options (known after apply)

      + maintenance_options {
          + auto_recovery = "default"
        }

      + metadata_options {
          + http_endpoint               = "enabled"
          + http_protocol_ipv6          = "disabled"
          + http_put_response_hop_limit = 2
          + http_tokens                 = "required"
          + instance_metadata_tags      = "disabled"
        }

      + network_interface (known after apply)

      + primary_network_interface (known after apply)

      + private_dns_name_options {
          + enable_resource_name_dns_a_record    = true
          + enable_resource_name_dns_aaaa_record = false
          + hostname_type                        = "ip-name"
        }

      + root_block_device {
          + delete_on_termination = true
          + device_name           = (known after apply)
          + encrypted             = false
          + iops                  = 3000
          + kms_key_id            = (known after apply)
          + tags_all              = (known after apply)
          + throughput            = 125
          + volume_id             = (known after apply)
          + volume_size           = 10
          + volume_type           = "gp3"
        }

      + secondary_network_interface (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

# 5. Execute terraform import, which will add config to state file

$  terraform import aws_instance.webserver i-0bc5dc8c2bdcf7d78
```
aws_instance.webserver: Importing from ID "i-0bc5dc8c2bdcf7d78"...
aws_instance.webserver: Import prepared!
  Prepared aws_instance for import
aws_instance.webserver: Refreshing state... [id=i-0bc5dc8c2bdcf7d78]

Import successful!

The resources that were imported are shown above. These resources are now in
your Terraform state and will henceforth be managed by Terraform.

```

# 6. Execute terraform plan and see that no resources are added.

$ terraform plan 

```
aws_instance.webserver: Refreshing state... [id=i-0bc5dc8c2bdcf7d78]

No changes. Your infrastructure matches the configuration.

Terraform has compared your real infrastructure against your configuration and found
no differences, so no changes are needed
```
