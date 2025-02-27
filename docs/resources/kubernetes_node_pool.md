---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "civo_kubernetes_node_pool Resource - terraform-provider-civo"
subcategory: ""
description: |-
  Provides a Civo Kubernetes node pool resource. While the default node pool must be defined in the civo_kubernetes_cluster resource, this resource can be used to add additional ones to a cluster.
---

# civo_kubernetes_node_pool (Resource)

Provides a Civo Kubernetes node pool resource. While the default node pool must be defined in the `civo_kubernetes_cluster` resource, this resource can be used to add additional ones to a cluster.

## Example Usage

```terraform
# Query xsmall instance size
data "civo_instances_size" "xsmall" {
    filter {
        key = "type"
        values = ["kubernetes"]
    }

    sort {
        key = "ram"
        direction = "asc"
    }
}

# Create a cluster
resource "civo_kubernetes_cluster" "my-cluster" {
    name = "my-cluster"
    num_target_nodes = 1
    target_nodes_size = element(data.civo_instances_size.xsmall.sizes, 0).name
}

# Add a node pool
resource "civo_kubernetes_node_pool" "front-end" {
   cluster_id = civo_kubernetes_cluster.my-cluster.id
   num_target_nodes = 1
   region = "LON1"
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- **cluster_id** (String) The ID of your cluster
- **region** (String) The region of the node pool, has to match that of the cluster

### Optional

- **id** (String) The ID of this resource.
- **num_target_nodes** (Number) the number of instances to create (optional, the default at the time of writing is 3)
- **target_nodes_size** (String) the size of each node (optional, the default is currently g3.k3s.medium)

## Import

Import is supported using the following syntax:

```shell
# using cluster_id:node_pool_id
terraform import civo_kubernetes_node_pool.my-pool 1b8b2100-0e9f-4e8f-ad78-9eb578c2a0af:502c1130-cb9b-4a88-b6d2-307bd96d946a
```
