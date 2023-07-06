# Creating AWS S3-bucket as the remote backend for Terraform

Configuring a backend for any kind of Terraform project is always recommended. Backends are primarily meant to manage the information contained in the state files. State files hold the mapping of the Terraform configuration with their real-world deployments. The state files are created at the very first execution of the Terraform configuration. All the runs after that will always be validated with the same state files to make sure the intended changes are applied.

## Features

* Secure storage
* Locking

