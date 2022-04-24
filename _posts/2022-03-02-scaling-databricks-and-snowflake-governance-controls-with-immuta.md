---
layout: post
title: scaling Databricks and Snowflake governance controls with Immuta
tags:
- data governance
- data access control
- data security
- business process
- scaling
- Immuta
- Databricks
- Snowflake
- use cases
- tech
published: true
---
In most enterprise organizations, I see a need to have Databricks and Snowflake coexist for different
users and workloads. With Immuta, we can build on top of the existing governance control primitives
provided by both platforms to write policy in human-readable form, in one place instead of two.

Snowflake comes out of the data warehouse tradition: a central repository for fast access
analytics data which has been structured and filtered.
SQL wizards ply their trade crafting queries to power dashboards.

Databricks was born from data lake ideas: a repository of
large amounts of raw / unstructured / semi-structured data with data engineers building pipelines
to Extract-Transform-Load data.

### Databricks controls
Today, Databricks provides
[permissions to clusters](https://docs.databricks.com/security/access-control/cluster-acl.html) and
[permissions on tables](https://docs.databricks.com/security/access-control/table-acls/index.html).
Databricks is currently building [Unity Catalog](https://databricks.com/product/unity-catalog),
with Immuta's advisement,
which will allow DBAs and lakehouse administrators the ability to write `GRANT` statements
to manage column and row access, based on tagging done in Databricks. Stuff like:

```sql
GRANT SELECT ON events TO data_engineering;
GRANT SELECT(timestamp, location) ON events TO analysts;
 ```

### Snowflake controls
Today, Snowflake supports conditional masking of both
[columns](https://docs.snowflake.com/en/user-guide/security-column-intro.html#apply-a-conditional-masking-policy-on-a-column) and
[rows](https://docs.snowflake.com/en/user-guide/security-row-intro.html#apply-a-row-access-policy-to-a-table-or-view)
on tables and views. To create a column masking policy for social insurance/security numbers:

```sql
create or replace masking policy social_mask as (val string) returns string ->
    case
    when current_role() in ('HR') then val
    else 'REDACTED BY POLICY'
    end;

create or replace table employee_info (social_number string masking policy social_mask) using (social_number, visibility);
```

We will continue to see advances in platform support for data governance controls.
In addition to being a Snowflake Premier partner, Immuta will continue to jointly develop data governance
solutions with Snowflake as part of the
[Data Governance Accelerated Program](https://www.businesswire.com/news/home/20211116006058/en/Immuta-Joins-Snowflake%E2%80%99s-Data-Governance-Accelerated-Program).

### Beyond GRANTs and SQL
If you are in one ecosystem or the other,
doing these sorts of things might be possible for a small data organization.
The trouble comes when you get into data mesh architectures with geographically dispersed domain/experts.
Every domain should be able to onboard their own data. Every domain knows what policies _should_ be
enforced on the data they have added. Users will work across or switch domains.

This is where Immuta comes in to leverage platform primitives, for fine-grained access control at scale:
<https://www.immuta.com/capabilities/data-access-control/>

### Create an Immuta instance
Two different avenues exist right now to spin up an Immuta instance:
- Request a 14-day free trial directly from Immuta: <https://www.immuta.com/try/>
- Create a 14-day free trial through
[Snowflake partner connect](https://docs.snowflake.com/en/user-guide/ecosystem-partner-connect.html): <https://www.immuta.com/articles/automated-snowflake-access-control-on-partner-connect/>

### Integrating Immuta into Snowflake
If you created an Immuta instance through Snowflake partner connect,
there should not be any more steps you need to complete. For any other install ...

You need to connect the
[Snowflake native access pattern](https://documentation.immuta.com/SaaS/1-installing-and-configuring/installation/access-pattern-installation/snowflake/)
to do the enforcement and
[connect data sources](https://documentation.immuta.com/SaaS/4-connecting-data/creating-data-sources/storage-technologies/general/snowflake-tutorial/)
that you want Immuta to do enforcement on.
Be sure to add some members to the new data source
[directly](https://documentation.immuta.com/SaaS/prologue/immuta-ui/data-source-page/#members-tab) or through
[subscription policies](https://documentation.immuta.com/SaaS/3-writing-global-policies-for-compliance/global-policy-builder/subscription-policy-tutorial/)!

If using the automatic Snowflake native access pattern setup, the Snowflake role you utilize for setup will need:

```sql
GRANT CREATE DATABASE ON ACCOUN TO ROLE immuta_bootstrap_role WITH GRANT OPTION;
GRANT CREATE ROLE ON ACCOUNT TO ROLE immuta_bootstrap_role WITH GRANT OPTION;
GRANT CREATE USER ON ACCOUNT TO ROLE immuta_bootstrap_role WITH GRANT OPTION;
GRANT MANAGE GRANTS ON ACCOUNT TO ROLE immuta_bootstrap_role;
```

Immuta will create/manage all Snowflake conditional masking policies through the `PUBLIC` Snowflake role
so you will need to opt the `PUBLIC` role into the db you want to enforce policy on through Immuta:

```sql
GRANT USAGE ON DATABASE DB_TO_ENFORCE_PERMS TO ROLE PUBLIC;
GRANT USAGE ON SCHEMA DB_TO_ENFORCE_PERMS.PUBLIC TO ROLE PUBLIC;
GRANT SELECT ON ALL TABLES IN SCHEMA DB_TO_ENFORCE_PERMS.PUBLIC TO ROLE PUBLIC;
```

`GRANT`ing to `PUBLIC` is only required if you are using Snowflake native controls,
which requires Snowflake Enterprise Edition.

### Integrating Immuta into Databricks
The long term vision with Unity Catalog is that Databricks will support the necessary SQL syntax for
creating conditional masking policies at a base level. For now though, Immuta enforcement occurs as a plugin to
a Databricks cluster and hits Immuta to inspect entitlements and add query parameters prior to queries
going out to the metastore query planner.

Immuta has made this setup easier recently through the option to automatically push the Immuta init script
and cluster policies into Databricks for use in constructing Immuta-governed Databricks clusters:
<https://documentation.immuta.com/2021.5/1-installing-and-configuring/installation/access-pattern-installation/databricks/installation/simplified/>

For those of you who want to understand full-manual, I'll break that down here:

First, upload the Immuta Databricks init script to DBFS.
Then, choose a [supported Databricks runtime](https://documentation.immuta.com/SaaS/1-installing-and-configuring/installation/access-pattern-installation/databricks/installation/#supported-databricks-runtimes).
Select to create a new Databricks cluster and point the cluster at the Immuta instance through the
_Spark_ and _Init Scripts_ sections of the _Advanced options_.

Spark config:

```sh
spark.databricks.cluster.profile serverless
spark.databricks.isv.product Immuta
spark.driver.extraJavaOptions -Djava.security.manager=com.immuta.security.ImmutaSecurityManager -Dimmuta.security.manager.classes.config=file:///databricks/immuta/allowedCallingClasses.json -Dimmuta.spark.encryption.fpe.class=com.immuta.spark.encryption.ff1.ImmutaFF1Service
spark.databricks.pyspark.enableProcessIsolation true
spark.executor.extraJavaOptions -Djava.security.manager=com.immuta.security.ImmutaSecurityManager -Dimmuta.security.manager.classes.config=file:///databricks/immuta/allowedCallingClasses.json -Dimmuta.spark.encryption.fpe.class=com.immuta.spark.encryption.ff1.ImmutaFF1Service
spark.databricks.repl.allowedLanguages python,sql
```

Environment variables:

```sh
IMMUTA_SPARK_ACL_ENABLED=true
IMMUTA_BASE_URL=https://myinstance.demo.immuta.com
IMMUTA_INIT_HTTPS_USER=immuta_init
IMMUTA_INIT_OBSCURED_COMMANDS_URI=https://myinstance.demo.immuta.com/databricks/artifacts/obscuredCommands.yaml
IMMUTA_SYSTEM_API_KEY=[IMMUTA HDFS KEY]
IMMUTA_USER_MAPPING_IAMID=bim
IMMUTA_INIT_TLS=false
IMMUTA_SPARK_DATABRICKS_ALLOW_NON_IMMUTA_WRITES=false
IMMUTA_INIT_ALLOWED_CALLING_CLASSES_URI=https://myinstance.demo.immuta.com/databricks/artifacts/allowedCallingClasses.json
IMMUTA_INIT_HTTPS_PASSWORD=[IMMUTA HDFS KEY]
IMMUTA_SPARK_DATABRICKS_ALLOW_NON_IMMUTA_READS=false
IMMUTA_INIT_JAR_URI=https://myinstance.demo.immuta.com/databricks/artifacts/immuta_plugin.jar
```

Init Scripts:

![](/public/images/databricks_cluster_init.png)

### Create one policy
The final step is to apply a single (global) policy which affects the data views, at query time,
across both platforms. For an example:
[Global Policy Implementation using Data Attributes](https://www.youtube.com/watch?v=ZzCQ7n8G0_Q).
