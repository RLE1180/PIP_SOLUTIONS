# Chef Repository README

## Description

This repository contains Chef configurations for managing server infrastructure and applications using Chef recipes, cookbooks, and environments.

## Getting Started

### Prerequisites

- [Chef Development Kit (ChefDK)](https://downloads.chef.io/tools/chefdk)
- Access to a Chef Server

### Setup Instructions

1. **Clone the Repository:**

    ```bash
    git clone <repository_url>
    cd chef-repo
    ```

2. **Install Dependencies:**

    Ensure ChefDK is installed, and run:

    ```bash
    chef exec bundle install
    ```

3. **Create Environments and Cookbooks:**

    - Define environments in the `environments/` directory.
    - Create cookbooks using `chef generate cookbook <cookbook_name>`.

4. **Upload Cookbooks and Environments:**

    Use `knife` to upload cookbooks and environments to the Chef Server:

    ```bash
    knife cookbook upload <cookbook_name>
    knife environment from file environments/<environment_name>.json
    ```

5. **Bootstrap Nodes:**

    Bootstrap nodes with Chef using `knife bootstrap`:

    ```bash
    knife bootstrap <node_IP_or_hostname> -x <username> -P <password> --sudo -r 'recipe[<cookbook_name>::<recipe_name>]' -E <environment_name>
    ```

6. **Verify Configuration:**

    SSH into nodes and verify the applied configurations as per the README instructions.

## Troubleshooting

### Common Issues and Solutions

- **Issue 1:** Error encountered during cookbook upload.
  **Solution:** Check network connectivity and permissions. Ensure proper syntax in metadata.rb file.

- **Issue 2:** Bootstrap command fails with SSH connection error.
  **Solution:** Verify SSH credentials, check firewall settings, and ensure SSH access to the node.


