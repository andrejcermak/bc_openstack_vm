# OpenStack VM

Deploy an interactive Ubuntu virtual machine on OpenStack cloud infrastructure via Open OnDemand.

## Overview

This Open OnDemand Batch Connect app launches Ubuntu VMs on OpenStack cloud infrastructure. Users can select their project, choose a node flavor, and provide an SSH public key for secure access to the deployed VM.

## Features

- **Dynamic Project Selection**: Automatically loads available OpenStack projects from the configured cloud instance
- **Flavor-Based Sizing**: Choose from available VM flavors with project-specific options
- **SSH Key Authentication**: Secure access via SSH public key deployment
- **Error Logging**: Displays error logs in the session view for troubleshooting

## Requirements

### Server-Side Requirements

- Open OnDemand 4.2+
- **Coder Cluster**: This app uses with the Coder resource manager for VM lifecycle management. See the [Coder documentation](https://osc.github.io/ood-documentation/latest/installation/resource-manager/coder.html) for setup and configuration details.

### Compute Node Requirements

- No special requirements - VMs are deployed directly to OpenStack

### User Requirements

- SSH key pair for VM access

## Installation

1. Copy this repository to your Open OnDemand apps directory:

```bash
cd /etc/ood/config/apps
git clone <repository-url> bc_openstack_vm
```

## Configuration

### Site-Specific Configuration

Edit [`form.yml.erb`](form.yml.erb) and [`submit.yml.erb`](submit.yml.erb) to customize the following values:

| Parameter | Location | Description | Default |
|-----------|----------|-------------|---------|
| `openstack_instance` | [`form.yml.erb:6`](form.yml.erb#L6) | OpenStack Horizon/API endpoint | `brno.openstack.cloud.e-infra.cz` |
| `token_file` | [`form.yml.erb:7`](form.yml.erb#L7) | Path for OAuth token cache | `/tmp/#{user}-os-token.json` |
| `cluster_name` | [`form.yml.erb:8`](form.yml.erb#L8) | Target Coder cluster name | `coder` |
| `coder_template_version_id` | [`form.yml.erb:9`](form.yml.erb#L9) | Coder workspace template version ID | `NaN` |
| `coder_org_id` | [`form.yml.erb:10`](form.yml.erb#L10) | Coder organization ID | `NaN` |

### Customizing the OpenStack Instance

To use a different OpenStack cloud, modify line 5 in [`form.yml.erb`](form.yml.erb):

```ruby
openstack_instance = "your.openstack.instance.cz"
```

### Adding Custom Flavors

Flavors are automatically discovered from the OpenStack API. To filter or modify available flavors, edit the `flavors` processing in [`form.yml.erb`](form.yml.erb).

## Usage

1. Navigate to the app in your Open OnDemand dashboard
2. Select your OpenStack project from the dropdown
3. Choose the desired VM flavor
4. Paste your SSH public key
5. Click "Launch" to deploy the VM
6. Use the provided SSH command to connect to your VM

## Troubleshooting

### Common Issues

#### Empty Project Dropdown
- Verify your OpenStack credentials are valid
- Check network connectivity from the OOD host to the OpenStack API

#### VM Deployment Fails
- Confirm the selected flavor is available in your project
- Check that your project has sufficient quota
- Review OpenStack service status (Nova, Neutron)

#### Token File Errors
- Ensure the token file path is writable
- Tokens are generated using the pun hook
- Check disk space in `/tmp` directory
- Clear stale tokens: `rm /tmp/<username>-os-token.json`

### Viewing Error Logs

Click the "Show error logs" button in the session view to diagnose issues. Error logs include:
- API connection failures
- Authentication errors
- Resource provisioning failures

## Known Limitations
?

## Testing

This app has been tested with:
- Open OnDemand 4.2

## Support

For issues specific to this app, open an issue in the repository. For Open OnDemand platform issues, visit the [Open OnDemand Discourse](https://discourse.openondemand.org/).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
