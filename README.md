DocumentDB Terraform Action
============================
Deploy an AWS DocumentDB using Terraform.


Prerequisites
-----

- Configure AWS Credential action must be executed before this action  
https://github.com/aws-actions/configure-aws-credentials 
- Terraform Project Action must be executed before this action  
https://github.com/aplaceformom/terraform-project-base-action
- DocumentDB master password must be stored in the enviornment's Credstash's default credential store.


Usage
-----

```yaml
  - name: 'DocumentDB Deploy'
      uses: aplaceformom/terraform-docdb-action@master
      with:
        cluster_name: example
        username: example_admin
        credstash_docdb_password: example_credstash_key
        subnet_ids: ${{ steps.project.outputs.subnet_id_private }}
        instance_count: 2
```


Inputs
-----

### destroy
Runs Terraform destroy to remove resources created by this action'
- required: false
- default: false

### deploy
Runs Terraform apply to create/update resources created by this action
- required: false
- default: true

### plan
Runs Terraform plan to check the changes necessary to achieve the desired state
- required: false
- default: true

### cluster_name
- DocumentDB cluster name
- required: true

### username
- Username for the DocumentDB access. Hyphens are not allowed.
- required: true

### credstash_docdb_password
- Credstash secret name/key. Used to retrieve the stored password for the DocumentDB cluster
- required: true

### parameter_group
- A cluster parameter group to associate with the cluster
- required: false

### security_group_ids
- A comma separated list of security group IDs associated with the cluster
- required: false

### port
- The port on which the DB accepts connections
- required: false
- default: 27017

### storage_encrypted
- Specifies whether the DB cluster is encrypted
- required: false
- default: false
### apply_immediately
- Specifies whether any cluster modifications are applied immediately, or during the next maintenance window. APPLY IMMEDIATELY WILL CAUSE OUTAGE
- required: false
- default: false

### backup_window
- The daily time range during which automated backups are created in UTC. e.g. 04:00-09:00
- required: false
- default: '07:00-11:00'

### backup_retention
- The days to retain backups for
- required: false
- default: 1

### instance_count
- Number of instances to create and join to the cluster
- required: false
- default: 1

### instance_class
- The instance class to use. Supported instance classes: https://docs.aws.amazon.com/documentdb/latest/developerguide/db-instance-classes.html#db-instance-class-specs
- required: false
- default: db.r5.large

### subnet_ids
- A comma separated list of subnet IDs
- required: true

### aws_assume_role
- ARN of the assumable role to execute Terraform against the environment. (Required if this is the first terraform deploy action used in the workflow)
- required: false

### aws_external_id
- AWS external ID required to grant access to role. (Required if this is the first terraform deploy action used in the workflow)
- required: false

Outputs
-------

|         Context            |              Description                |
|----------------------------|-----------------------------------------|
| documentdb_name            | Name of the created documentdb cluster  |
| documentdb_arn             | ARN of the created documentdb cluster   |
| documentdb_endpoint        | The DNS address of the DocDB instance   |
| documentdb_reader_endpoint | A read-only endpoint for the DocDB cluster, automatically load-balanced across replicas |


To Do
-------
- Add feature to generate password and push to credstash
