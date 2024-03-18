## Setting Up Twingate Client
Coder Templates are essentially Terraform configuration files, we can set up Twingate using these templates by manipulate the Terraform. 

### Connect using a regular Twingate account
For other types of templates, e.g. Kubernetes, modify the Terraform configuration file as follows
1. For container based solution, make sure the container is `priviledged`
2. Create Terraform Variable `twingate_tenant_name` by adding the following section in the Coder template Terraform
```hcl
variable "twingate_tenant_name" {
  type        = string
  description = "Twingate Tenant Name"
  default = "acme"
}
```
3. Create environmental variable `TWINGATE_TENANT_NAME`, for instance, for Coder starter Kubernetes template, appended the section below to the Terraform resource `kubernetes_deployment.main.spec.template.spec.container`
```hcl
env {
name  = "TWINGATE_TENANT_NAME"
value = var.twingate_tenant_name
}
```
4. Download and setup Twingate client by modifying the startup script, for instance, for Coder starter Kubernetes template, appended the section below to the Terraform resource `coder_agent.main.startup_script`
```shell
curl -s https://binaries.twingate.com/client/linux/install.sh | sudo bash
sudo -E bash -c 'echo $TWINGATE_TENANT_NAME > /etc/twingate/network.conf'
```
5. Go to the Setting page of the template and set your Twingate Tenant Name in Variable, e.g. for `acme.twingate.com`, insert `acme`
6. Create workspace and open the workspace terminal. run `twingate status`
    1. If status is `authenticating` you should follow the URL displayed to authenticate
    2. If status is `not running`, execute `twingate start` followed by `/usr/bin/twingate-notifier console` and follow the URL displayed to authenticate

### Connect using a Twingate service account
For other types of templates, e.g. Kubernetes, modify the Terraform configuration as follows
1. Generate a Twingate service account key
2. For container based solution, make sure the container is `priviledged`
3. Create Terraform Variable `twingate_service_key` by adding the following section in the Coder template Terraform
```hcl
variable "twingate_service_key" {
  type        = string
  description = "Twingate Service Key"
  sensitive   = true
}
```
3. Create environmental variable `TWINGATE_SERVICE_KEY`, for instance, for Coder starter Kubernetes template, appended the section below to the Terraform resource `kubernetes_deployment.main.spec.template.spec.container`
```hcl
env {
name  = "TWINGATE_SERVICE_KEY"
value = var.twingate_service_key
}
```
4. Download and setup Twingate client by modifying the startup script, for instance, for Coder starter Kubernetes template, appended the section below to the Terraform resource `coder_agent.main.startup_script`
```shell
curl -s https://binaries.twingate.com/client/linux/install.sh | sudo bash
echo $TWINGATE_SERVICE_KEY | sudo twingate setup --headless=-
twingate start
```
5. Go to the Setting page of the template and set your Twingate Service Key in Variable
6. Create workspace and open the workspace terminal. run `twingate status` - it should return `online`
