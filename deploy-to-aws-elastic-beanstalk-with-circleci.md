# Elastic Beanstalk (Console)

---

## Glossary

+ **JAR** == *Java Archive*

---

## How-to

---

### How-to Create an Application *Source Bundle*

> + Consist of a single ZIP file or WAR file (you can include multiple WAR files inside your ZIP file)
> + Not exceed 500 MB
> + Not include a parent folder or top-level directory (subdirectories are fine)

```bash
my_app.zip
├── .ebextensions
│   └── env.config
├── my_app.jar
├── static/
└── README.md
```

[docs][9]

#### Creating a Source Bundle from a CLI

Using the *zip* command including **hidden** **files** and **folders** 

`~/myapp$ zip ../my_app.zip -r * .[^.]*`

#### Creating a source bundle with Git

*git archive* **only includes** files that are stored **in git**, and **excludes** **ignored** files and **git files**

`git archive -v -o myapp.zip --format=zip HEAD`

---

### How-to use the Elastic Beanstalk Java SE platform

> If you only provide a **single** *JAR file* for your application source (on its own, not within a *source bundle*), *Elastic Beanstalk* **renames** your *JAR file* to **application.jar**[^4], and then runs it using `java -jar application.jar`.

> To configure the processes that run on the server instances in your environment, include an optional *Procfile* in your *source bundle*. A *Procfile* is required if you have more than one JAR in your *source bundle* root, or if you want to customize the java command to set JVM options.

> We recommend that you always **provide** a *Procfile* in the *source bundle* alongside your application. This way you precisely control which processes *Elastic Beanstalk* runs for your application and which arguments these processes receive.

[^4]: /var/app/current/application.jar started by webapp in /var/app/current

[docs][8]

---

### How to Deploy Java Application JAR to AWS Beanstalk with Gradle

1. st
1. st
1. st

[Medium.com][4]

--- 

### How-to create Configuration files

> *Configuration files* are **YAML**- or **JSON**-formatted documents with a `.config` file **extension** that you place in a **folder** named `.ebextensions` and **deploy** in the **root** of your *application source bundle*.

+ keys and values are **case sensitive**.
+ some characters that need to be **escaped**
    + \ — `\\`
    + ' — `\'`
    + " — `\"`

> When you are **developing** or **testing** new *configuration files*, launch a **clean** *environment* running the **default** *application* and deploy to that. **Poorly formatted** *configuration files* will cause a new environment launch to **fail unrecoverably**.

> Use each **key** only **once** in each *configuration* **file**. **Combine** duplicate sections into a single one, or place them in **separate** configuration **files**.

+ [Configuring Elastic Beanstalk environments (advanced)][5]
    + [General options for all environments][7]
+ [Environment properties and other software settings][6]
    + [Software setting namespaces][10]
    
---

### How-to use Load Balancer

> Elastic Beanstalk creates and configures the load balancer to **listen** for HTTP traffic on **port 80** and **forward** this traffic to instances on **the same port**[^2].

[^2]: From which it is forwarded further by *nginx* server reverse proxy to the **application port** (default **5000** for Java SE)
---

## Snippets

---

##### environment configuration

filepath: `.ebextensions/*.config`

```yml
my_section:
  my:scope:
    MY_KEY: my_value
```

sections: `option_settings`, `packages`, `sources`, `files`, `users`, `groups`, `commands`, `container_commands`, and `services`
---

##### accessing environment properties

```java
// Java SE – System.getenv
String endpoint = System.getenv("API_ENDPOINT");
```
```java
//Tomcat – System.getProperty
String endpoint = System.getProperty("API_ENDPOINT");
```
[Accessing environment properties][11]

---

##### basic Buildfile

```bash
build: gradle assembly:assembly -DdescriptorId=jar-with-dependencies
```
[docs][3]

##### basic Procfile

```bash
web: java -jar path/to/my_app.jar
```
[docs][2]

---

## Examples 

---

##### loadbalancer config

filepath: `.abextensions/load-balancer.config`

```yml
option_settings:
  aws:elasticbeanstalk:environment:proxy:staticfiles:
    /static: static 
    # e.g. ....com/static/static.html
  aws:elasticbeanstalk:environment:
    LoadBalancerType: network
```

specifies */static* to serve files at *subdomain* `.eleasticbeanstalk.com/static` from a `static` folder at the **top level** of your *source bundle*. 

##### environment config

filepath: `.ebextensions/env.config`
e.g. `env`, `environment`,`development`,...

```yml
option_settings:
  aws:elasticbeanstalk:application:environment:
    PORT: 5000
    USER_USERNAME: Matej
    USER_PASSWORD: 51c0e357-e949-4f04
```

[docs][1]

---

##### Buildfile

```
build: gradle assembly:assembly -DdescriptorId=jar-with-dependencies
```

##### Procfile

```bash
web: java -jar build/libs/my_app.jar
```

---

##### DB credentials

> Launch a DB instance **with** *Amazon RDS* completely **independent** of *Elastic Beanstalk* and your *EB environments*. This means that you can use **any** DB **engine** and instance **type** supported by *Amazon RDS*, even those that aren't used by *Elastic Beanstalk*.
> Or start with a database that was previously **created by** *Elastic Beanstalk* (and optionally subsequently *decoupled*).

[Using Elastic Beanstalk with Amazon RDS][12]
[Launching and connecting to an external Amazon RDS instance in a default VPC][13]

JDBC connection definition:
```bash
jdbc:,driver://hostname:port/schema?user=username&password=password
```
```bash
spring.datasource.url=jdbc:mysql://${RDS_HOSTNAME}:${RDS_PORT}/${RDS_DB_NAME}?user=${RDS_USERNAME}&password=${RDS_PASSWORD}
```

| Environment Variable | Description                                        | Amazon RDS Console                   |
|----------------------|----------------------------------------------------|--------------------------------------|
| `RDS_HOSTNAME`       | DB instance hostname                               | Connectivity & Security/**Endpoint** |
| `RDS_PORT`           | DB instance connections port (**varies** by engine)| Connectivity & Security/**Port**     |
| `RDS_DB_NAME`        | Schema name (`ebdb`)                               | Configuration/**DB Name**            |
| `RDS_USERNAME`       | DB username                                        | Configuration/**Master username**    |
| `RDS_PASSWORD`       | DB password                                        | *Not available*                      |

> To allow the *Amazon EC2* instances in your environment to connect to an outside database, **configure** an **additional** *security group* for the *Auto Scaling group* that's associated with your **environment**. You can **attach** the same *security group* that's attached to your **database** instance. Or, you can use a **separate** *security group*. If you attach a different *security group*, you must configure the *security group* that's attached to your database to allow inbound access from this *security group*.

---

# Elastic Beanstalk CLI

---

## Glossary

---

### .ebignore file

> You can tell the *EB CLI* to **ignore** certain **files** in your project directory by adding the file *.ebignore* to the directory. This file works like a *.gitignore* file. When you **deploy** your project directory to *Elastic Beanstalk* and create a new application version, the *EB CLI* **doesn't include** files specified by *.ebignore* in the *source bundle* that it creates.

> If *.ebignore* **isn't** present, but *.gitignore* **is**, the *EB CLI* ignores files specified in *.gitignore*.
> If *.ebignore* is **present**, the *EB CLI* **doesn't** read *.gitignore*.

When *.ebignore* is **present**, the *EB CLI* **doesn't** use *git* commands to create your *source bundle*. This means that *EB CLI* ignores files specified in *.ebignore*, and includes all other files. In particular, it includes uncommitted source files.

---

### Named profiles

If you store your credentials as a named profile in a credentials or config file, you can use the --profile option to explicitly specify a profile

`eb init --profile another_profile`

When this variable is set, the EB CLI reads credentials from the specified profile instead of default or eb-cli.

`export AWS_EB_PROFILE=default_profile`

---

## How-to

---

### How-to configure AIM User for EB application

1. add user *eb-admin* to a *eb-admins* group with permissions in IAM Console:
    + *AdministratorAccess-AWSElasticBeanstalk*[^0]
    + *AmazonEC2FullAccess*[^1]
    + *AmazonS3FullAccess*[^2]
    + *AWSCodeCommitFullAccess*[^3]

---

### How-to configure EB CLI configuration settings and precedence

> The EB CLI uses a provider chain to look for AWS credentials in a number of different places, including system or user environment variables and local AWS configuration files.

The EB CLI looks for credentials and configuration settings in the following order:

+ *Command line options* – Specify a named profile by using `--profile` to **override** **default** settings.
+ *Environment variables* – `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.
+ The *AWS credentials file* – At `~/.aws/credentials`. This file can contain **multiple** *named profiles* in **addition** to a *default profile*.
+ The *AWS CLI* configuration file – At` ~/.aws/config`. This file can contain a *default profile*, *named profiles*, and *AWS CLI***–specific** configuration parameters **for each**.

Legacy *EB CLI* configuration file – At `~/.elasticbeanstalk/config`.

*Instance profile credentials* – These credentials can be used on *Amazon EC2* instances with an assigned instance role, and are delivered through the Amazon EC2 metadata service. The instance profile must have **permission** to use Elastic Beanstalk.

If the credentials file contains a named profile with the name **"eb-cli"**, the EB CLI will **prefer** that profile over the **default** profile. If no profiles are found, or a profile is found but does not have **permission** to use Elastic Beanstalk, the EB CLI prompts you to **enter** *keys*.

[Configure the EB CLI][14]

---

### How-to Deploy

[^0]: quoted in the quickstart guide
[^1]: for storing keys during `eb init`
[^2]: for `eb deploy`
[^3]: for `codeCommit` (optional)


1. `eb init`
    + select no to 'use *CodeCommit*' option for the time being

1. use the *Access key pair* as credentials

1. specify *.ebignore* or use *.gitignore* or both (see the [.ebignore file](#ebignore-file) section)

1. `eb create <my_env>`

1. `eb use <my_env>`

1. `eb deply`

```
2023-10-05 19:28:37    ERROR   Instance deployment failed. For details, see 'eb-engine.log'.
2023-10-05 19:28:40    ERROR   [Instance: i-0bb87d255cc800cac] Command failed on instance. Return code: 1 Output: Engine execution has encountered an error..
2023-10-05 19:28:40    INFO    Command execution completed on all instances. Summary: [Successful: 0, Failed: 1].
2023-10-05 19:29:43    ERROR   Create environment operation is complete, but with errors. For more information, see troubleshooting documentation.
                                
ERROR: ServiceError - Create environment operation is complete, but with errors. For more information, see troubleshooting documentation.
```

## Snippets

---

##### deploying an artifact instead of the project folder

1. add EB CLI config file

    filepath: `.elasticbeanstalk/config.yml`

    ```bash
    deploy:
      artifact: path/to/buildartifact.zip # or .war or .jar
    ```
1. run `en deploy`

note that the file is **updated** by the `eb` commands!

---

## Examples

---

##### .elasticbeanstalk/config.yml

note that the file is updated by `eb` commands (e.g. `eb use <my_env>`)!

```yml
branch-defaults:
  develop:
    environment: elastic-beanstalk-demo-env
  main:
    environment: elastic-beanstalk-demo-prod

deploy:
  artifact: build/zips/new.zip

environment-defaults:
  elastic-beanstalk-demo-env:
    branch: null
    repository: null
global:
  application_name: elastic-beanstalk-demo
  default_ec2_keyname: aws-eb
  default_platform: Corretto 17 running on 64bit Amazon Linux 2023
  default_region: eu-central-1
  include_git_submodules: true
  instance_profile: null
  platform_name: null
  platform_version: null
  profile: eb-cli
  sc: git
  workspace_type: Application
```


---

# Circle CI

---
...
---

[1]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environment-cfg-staticfiles.html
[2]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-procfile.html
[3]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-buildfile.html
[4]: https://medium.com/agorapulse-stories/how-to-deploy-java-application-jar-to-aws-beanstalk-with-gradle-35343337febf
[5]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/beanstalk-environment-configuration-advanced.html
[6]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-cfg-softwaresettings.html
[7]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options-general.html
[8]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-platform.html
[9]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications-sourcebundle.html
[10]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-cfg-softwaresettings.html#environments-cfg-softwaresettings-configfiles
[11]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-cfg-softwaresettings.html#environments-cfg-softwaresettings-accessing
[12]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.RDS.html
[13]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/rds-external-defaultvpc.html
[14]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-configuration.html