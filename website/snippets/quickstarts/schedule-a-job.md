## Commit your changes

Now that you've built your customer model, you need to commit the changes you made to the project so that the repository has your latest code.

1. Click **Commit** and add a message. For example, "Add customers model, tests, docs."
2. Click **merge to main** To add these changes to the main branch on your repo.

## Create a deployment environment

1. In the upper left, select **Deploy**, then click **Environments**.
2. Click **Create Environment**.
3. Name your deployment environment. For example, "Production."
4. Add a target dataset, for example, "Analytics." dbt will build into this dataset. For some warehouses this will be named "schema."
5. Click **Save**.

## Create and run a job

Jobs are a set of dbt commands that you want to run on a schedule. For example, `dbt run` and `dbt test`.

As the `jaffle_shop` business gains more customers, and those customers create more orders, you will see more records added to your source data. Because you materialized the `customers` model as a table, you'll need to periodically rebuild your table to ensure that the data stays up-to-date. This update will happen when you run a job.

1. After creating your deployment environment, you should be directed to the page for new environment. If not, select **Deploy** in the upper left, then click **Jobs**.
2. Click **Create one** and provide a name, for example "Production run", and link to the Environment you just created.
3. Scroll down to "Execution Settings" and select **Generate docs on run**.
4. Under "Commands," add these commands as part of your job if you don't see them:
      * `dbt run`
      * `dbt test`
5. For this exercise, do _not_ set a schedule for your project to run &mdash; while your organization's project should run regularly, there's no need to run this example project on a schedule. Scheduling a job is sometimes referred to as _deploying a project_.
6. Select **Save**, then click **Run now** to run your job.
7. Click the run and watch its progress under "Run history."
8. Once the run is complete, click **View Documentation** to see the docs for your project.

:::tip
Congratulations 🎉! You've just deployed your first dbt project!
:::

### FAQs

<FAQ src="Runs/failed-prod-run" />

## Related content
- Learn more with [dbt Courses](https://courses.getdbt.com/collections)
- [dbt Cloud CI job](/docs/deploy/cloud-ci-job)
- [Job triggers](/docs/deploy/job-triggers)
- [Job notifications](/docs/deploy/job-notifications)
- [Source freshness](/docs/deploy/source-freshness)