---
title: "Job commands"
id: "job-commands"
description: "How to use dbt commands to set tasks for your dbt Cloud jobs."
---

A dbt Cloud production job allows you to set up a system to run a dbt job and job commands on a schedule, rather than running dbt commands manually from the command line or [IDE](/docs/get-started/develop-in-the-cloud). Each job allows you to:

- Configure job commands
- View job run details, including timing, artifacts, and detailed run steps
- Access logs to view or help debug issues and historical invocations of dbt
- Set up notifications, and [more](/docs/deploy/deployments#dbt-cloud)!

## Job command types

Job commands are specific tasks executed by the job, and you can configure them seamlessly by either adding [dbt commands](/reference/dbt-commands) or using the checkbox option in the **Commands** section. 

When using configuring job commands, expect different outcomes when adding the command as a run step compared to selecting a checkbox.

<Lightbox src ="/img/docs/dbt-cloud/using-dbt-cloud/job-commands.gif" title="Configuring checkbox and commands list"/>


### Built-in commands

Every job invocation includes [`dbt deps`](/reference/commands/deps), meaning you don't need to add it to the **Commands** list in your job settings. `dbt deps` pulls the most recent version of the dependencies listed in your `packages.yml` from git. 

**Outcome** &mdash; During a job run, the commands are “chained” together. This means if `dbt deps` fails, then the subsequent commands aren't executed a and the entire job fails with a non-zero exit code and "Error" job status.


<Lightbox src="/img/docs/dbt-cloud/using-dbt-cloud/fail-dbtdeps.jpg" title="A job run with a failed dbt deps run step"/>

### Checkbox commands

For every job, you have the option to select the [Generate docs on run](/docs/collaborate/build-and-view-your-docs) or [Run on source freshness](/docs/deploy/source-freshness) checkboxes. These checkboxes enable you to generate project docs or enable source freshness automatically.

**Outcome** &mdash; During a job run, if this particular step fails, the job can still be successful if all subsequent steps are successful. 

### Command list

You can add or remove as many [dbt commands](/reference/dbt-commands) as necessary for every job, however, you need to have at least one dbt command.
   
   * Use [selectors](/reference/node-selection/syntax) as a powerful way to select and execute portions of your project in a job run. If a selector doesn't match any models, the job run won't consider it a failure. For example, to run tests for one_specific_model, use the selector: `dbt test --select one_specific_model`
    
   *  TO FLESH OUT - IF THE SELECTOR DOESN'T MATCH NODES, WILL IT BE CONSIDERED? CANCELLED? CAN YOU PROVIDE AN EXAMPLE? NEED MORE INFO

**Outcome** &mdash;  During a job run, the commands are “chained” together. This means if one of the commands in the chain fails, then the next ones aren't executed. For example, if the 5th step fails (`dbt run --select state:modified+ --full-refresh --fail-fast`), then the subsequent commands aren't executed, and the entire job fails. The failed job returns a non-zero [exit code](https://docs.getdbt.com/reference/exit-codes) and "Error" job status:

<Lightbox src ="/img/docs/dbt-cloud/using-dbt-cloud/skipped-jobs.jpg" title="A job run with a failed run step and subsequent job failure"/>


## Job command failures

Job failures can mean different things for different commands. Here are some reasons why a job may fail:

1. **Failure at`dbt run`** &mdash; [`dbt run`](/reference/commands/run) executes compiled sql model files against the current target database. It will fail if there is an error in any of the built models. 
    - If a [`select`](/reference/node-selection/set-operators) matches multiple nodes and one of the nodes fails, then the job will have an exit code `1` and the subsequent command will fail. If you specified the [`—fail-fast`](/reference/global-configs#failing-fast) flag, then the first failure will stop the entire connection for any models that are in-progress.  
    - Tests on upstream resources prevent downstream resources from running and a failed test will skip them.
    - TODO WHAT IS THE EXIST CODE FOR A PARTIAL FAILE?

2. **Failure at `dbt test`** &mdash;  [`dbt test`](/reference/commands/test) runs tests defined on models, sources, snapshots, and seeds. A test can pass, fail, or warn depending on its [configuration](/reference/test-configs). Only an error stops the next step. 

3. **Failure at `dbt build`** &mdash; [`dbt build`](/reference/commands/build) runs models, tests, snapshots, and seeds. NEED MORE INFO ON THIS - what info do you want to convey? 

## Related docs
- [dbt commands](/reference/dbt-commands)
- [Job triggers](/docs/deploy/job-triggers)
- [Job notifications](/docs/deploy/job-notifications)
- [Source freshness](/docs/deploy/source-freshness)
- [Build and view your docs](/docs/collaborate/build-and-view-your-docs)