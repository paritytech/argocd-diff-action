# ArgoCD Diff with Teleport Auth

This GitHub Action allows you to show differences for applications in ArgoCD using Teleport authentication. It is tailored for CI/CD workflows to improve deployment pipelines and provides a detailed report of changes in a markdown format.

## Features

- Compares ArgoCD application states between the current branch and cluster.
- Secured Teleport-based authentication.
- Generates a markdown report of the diff results for review.
- Supports directory-scoped application diffs to optimize checks.

## Inputs

| Input                  | Description                                                                          | Required | Default                                 |
| ---------------------- |--------------------------------------------------------------------------------------| -------- | --------------------------------------- |
| `argocd_auth_token`    | ArgoCD authentication token (provided by the DevOps team).                           | Yes      |                                         |
| `argocd_version`       | Version of the ArgoCD CLI.                                                           | No       | `v2.11.4`                               |
| `changed_dirs`         | Newline or space-separated list of changed directories to limit diff scope.          | No       | `.`                                     |
| `teleport_proxy_server`| Teleport proxy server address.                                                       | No       | `teleport.parity.io:443`                |
| `teleport_token_name`  | Teleport token name for authentication. (Provided by the DevOps team; not a secret.) | Yes      |                                         |
| `teleport_app_name`    | Teleport application name (e.g., `argocd-prod`, `argocd-stg`).                       | Yes      |                                         |
| `teleport_bin`         | Version of the Teleport binary to use.                                               | No       | `teleport-v16.0.3-linux-amd64-bin.tar.gz` |
| `output_file`          | Path to the generated diff report file.                                              | No       | `/tmp/argocd-diff-report.md`            |

## Secrets

This action requires the following secret:

- `ARGOCD_AUTH_TOKEN`: Authentication token for ArgoCD (environment-specific).

## Usage

To integrate this action into your workflow, include the following step:

```yaml
- name: Run ArgoCD Diff with Teleport Auth
  uses: paritytech/argocd-diff-action@main
  with:
    argocd_auth_token: ${{ secrets.ARGOCD_AUTH_TOKEN }} # Set as a GitHub secret
    changed_dirs: "helm/dir1 helm/dir2" # Optional: Specify the directories for diff
    teleport_proxy_server: "teleport.example.com:443" # Adjust to your setup
    teleport_token_name: "your-teleport-token-name" # Provided by the DevOps team
    teleport_app_name: "argocd-prod" # Application name in Teleport
    argocd_version: "v2.11.4" # Optional: Specify ArgoCD CLI version
    teleport_bin: "teleport-v16.0.3-linux-amd64-bin.tar.gz" # Optional: Specify Teleport binary
    output_file: "/tmp/argocd-diff-report.md" # Optional: Specify custom report path
```

## Notes

- The `teleport_token_name` is not a secret. Request it from your DevOps team.
- The `secrets.ARGOCD_AUTH_TOKEN` must be configured as a repository secret in GitHub.
- Ensure the `teleport_proxy_server` and `teleport_app_name` align with your deployment environment.
- The generated report (`output_file`) contains a summary of diffs with detailed changes and statistics for easier review.
- Use the `changed_dirs` input to limit the scope of diffs to specific directories, reducing unnecessary checks.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
