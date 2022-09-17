# Terraform setup for creating a free VM on GCP running a minimal Flask server

## Requirements

* GCP account with activated billing
* gcloud CLI
* Terraform

## Preparation

Create a GCP project using the GCP web interface and edit `main.tf` to use your project ID.

Enable the Compute Engine and OS Login APIs using this URL: `https://console.cloud.google.com/flows/enableapi?apiid=compute.googleapis.com,oslogin.googleapis.com`

Set your default GCP region and zone with `gcloud compute project-info add-metadata --metadata google-compute-default-region=us-east4,google-compute-default-zone=us-east4-c`.

Run `gcloud init` to select your project and authorize.

## Applying the configuration

Run `terraform init` to initialize Terraform.

Run `terraform apply` to apply the configuration in `main.tf`. This will create the VM and additional networking resources for HTTP and SSH access.

The commands in `startup` are linked in `main.tf` and will automatically run from the OS (Debian) once the VM is up. They will download everything that's needed for a Flask server (using `apt` and `pip`), then create a single Python file and use it to start the web server on port 5000.

You can check the progress of this startup script with `gcloud compute instances get-serial-port-output flask-vm`.

## Accessing the web server

Once the Flask server is running, it can be accessed under the address listed at the end of the output from the previous `terraform apply` command. This output can also be checked with `terraform output`.

## Accessing the VM

The OS running on the VM can be accessed using SSH with `gcloud compute ssh flask-vm`.

## Cleanup

`terraform destroy` will delete all resources from your GCP project.