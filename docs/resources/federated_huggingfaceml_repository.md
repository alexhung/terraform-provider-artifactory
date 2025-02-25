---
subcategory: "Federated Repositories"
---
# Artifactory Federated Hugging Face ML Repository Resource

Creates a federated Hugging Face ML repository.

## Example Usage

```terraform
resource "artifactory_federated_huggingfaceml_repository" "my-huggingfaceml-federated" {
  key = "my-huggingfaceml-federated"

  member {
    url     = "http://tempurl.org/artifactory/my-huggingfaceml-federated"
    enabled = true
  }

  member {
    url     = "http://tempurl2.org/artifactory/my-huggingfaceml-federated-2"
    enabled = true
  }
}
```

## Argument Reference

The following attributes are supported, along with the [list of attributes from the local HuggingFace ML repository](local_huggingfaceml_repository.md):

* `key` - (Required) the identity key of the repo.
* `member` - (Required) The list of Federated members and must contain this repository URL (configured base URL
  `/artifactory/` + repo `key`). Note that each of the federated members will need to have a base URL set.
  Please follow the [instruction](https://www.jfrog.com/confluence/display/JFROG/Working+with+Federated+Repositories#WorkingwithFederatedRepositories-SettingUpaFederatedRepository)
  to set up Federated repositories correctly.
  * `url` - (Required) Full URL to ending with the repository name.
  * `enabled` - (Required) Represents the active state of the federated member. It is supported to change the enabled status of my own member. The config will be updated on the other federated members automatically.
  * `access_token` - (Optional) Admin access token for this member Artifactory instance. Used in conjunction with `cleanup_on_delete` attribute when Access Federation for access tokens is not enabled.
* `cleanup_on_delete` - (Optional) Delete all federated members on `terraform destroy` if set to `true`. Default is `false`. This attribute is added to match Terrform logic, so all the resources, created by the provider, must be removed on cleanup. Artifactory's behavior for the federated repositories is different, all the federated repositories stay after the user deletes the initial federated repository. **Caution**: if set to `true` all the repositories in the federation will be deleted, including repositories on other Artifactory instances in the "Circle of trust". This operation can not be reversed.
* `proxy` - (Optional) Proxy key from Artifactory Proxies settings. Default is empty field. Can't be set if `disable_proxy = true`.
* `disable_proxy` - (Optional, Default: `false`) When set to `true`, the proxy is disabled, and not returned in the API response body. If there is a default proxy set for the Artifactory instance, it will be ignored, too.

## Import

Federated repositories can be imported using their name, e.g.

```sh
terraform import artifactory_federated_huggingfaceml_repository.my-huggingfaceml-federated my-huggingfaceml-federated
```
