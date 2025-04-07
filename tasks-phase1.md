IMPORTANT ❗ ❗ ❗ Please remember to destroy all the resources after each work session. You can recreate infrastructure by creating new PR and merging it to master.

![img.png](doc/figures/destroy.png)

1. Authors:

   TBD - z10

   <https://github.com/mkowalewski1282/tbd-workshop-1/tree/master>

2. Follow all steps in README.md.

3. In boostrap/variables.tf add your emails to variable "budget_channels".

```hcl
variable "budget_channels" {
  type        = map(string)
  description = "Budget notification channels"
  default = {
    milosz-kowalewski = "01168891@pw.edu.pl"
    patryk-ploski     = "01168908@pw.edu.pl"
  }
}
```

4. From avaialble Github Actions select and run destroy on main branch.

![alt text](images/destroy_action.png)

5. Create new git branch and:
    1. Modify tasks-phase1.md file.

    2. Create PR from this branch to **YOUR** master and merge it to make new release.

    ![alt text](images/release_task_1.png)


6. Analyze terraform code. Play with terraform plan, terraform graph to investigate different modules.

    ***describe one selected module and put the output of terraform graph for this module here***

    ![alt text](images/terraform-graph-plan-dataproc.png)

7. Reach YARN UI

   ```hcl
   gcloud compute ssh tbd-cluster-m --project=tbd-2025l-9922 --zone=europe-west1-d --tunnel-through-iap -- -L 8088:localhost:8088
   ```

   ![alt text](images/yarn_ui.png)

8. Draw an architecture diagram (e.g. in draw.io) that includes:
    1. VPC topology with service assignment to subnets
    2. Description of the components of service accounts
    3. List of buckets for disposal
    4. Description of network communication (ports, why it is necessary to specify the host for the driver) of Apache Spark running from Vertex AI Workbech


    ![alt text](images/architecture_diagram.png)

9. Create a new PR and add costs by entering the expected consumption into Infracost
For all the resources of type: `google_artifact_registry`, `google_storage_bucket`, `google_service_networking_connection`
create a sample usage profiles and add it to the Infracost task in CI/CD pipeline. Usage file [example](https://github.com/infracost/infracost/blob/master/infracost-usage-example.yml)


```hcl
   version: 0.1
usage:
   google_artifact_registry.registry:
     storage_gb: 80

   google_storage_bucket.tbd_code_bucket:
     storage_gb: 200
     monthly_class_a_operations: 200
     monthly_class_b_operations: 500
     monthly_egress_data_gb: 170

   google_storage_bucket.tbd_data_bucket:
     storage_gb: 400
     monthly_class_a_operations: 100
     monthly_class_b_operations: 300
     monthly_egress_data_transfer_gb:
       same_continent: 90
       worldwide: 400
       asia: 20
       europe: 80

   google_service_networking_connection.private_vpc_connection:
    monthly_egress_data_transfer_gb:
      same_region: 250
      us_or_canada: 100
      europe: 70
      asia: 50
      south_america: 100
      oceania: 50
      worldwide: 200
```

   ![alt text](images/infracost_estimation.png)

10. Create a BigQuery dataset and an external table using SQL


    ![alt text](images/big_query_code.png)

    ![alt text](images/big_query_tasks_executions.png)

    ***why does ORC not require a table schema?***

11. Find and correct the error in spark-job.py
    Before fix:

    ![alt text](images/pyspark_before_fix.png)

    After changing data bucket name in spark-job.py file:

    ![alt text](images/pyspark_after_fix.png)

    ***describe the cause and how to find the error***

12. Add support for preemptible/spot instances in a Dataproc cluster
    Added below code to tbd-workshop-1/modules/dataproc/main.tf
    ```hcl
    preemptible_worker_config {
      num_instances = 1
    }
    ```

    ***place the link to the modified file and inserted terraform code***


