# Elastic Beanstalk

---

## Glossary

+ **JAR** == Java Archive

username: ebroot
password: hqbE3VueBE8AvKS

---

## How-to

---

### How to Deploy Java Application JAR to AWS Beanstalk with Gradle

1. st
1. st
1. st

[Medium.com][4]

--- 

### How-to ...use EB CLI...

1. add user *eb-admin* to a *eb-admins* group with permissions:
    + *AdministratorAccess-AWSElasticBeanstalk* [^0]
    + *AmazonEC2FullAccess* [^1]
    + *AmazonS3FullAccess* [^2]
    + *AWSCodeCommitFullAccess* [^3]
    
[^0]: quoted in the quickstart guide
[^1]: for storing keys during `eb init`
[^2]: for `eb deploy`
[^3]: for `codeCommit` (optional)


1. `eb init`
    + select no to *CodeCommit* for the time being

1. use the *Access key pair* as credentials

1. `eb create <my_env>`

```
2023-10-05 19:28:37    ERROR   Instance deployment failed. For details, see 'eb-engine.log'.
2023-10-05 19:28:40    ERROR   [Instance: i-0bb87d255cc800cac] Command failed on instance. Return code: 1 Output: Engine execution has encountered an error..
2023-10-05 19:28:40    INFO    Command execution completed on all instances. Summary: [Successful: 0, Failed: 1].
2023-10-05 19:29:43    ERROR   Create environment operation is complete, but with errors. For more information, see troubleshooting documentation.
                                
ERROR: ServiceError - Create environment operation is complete, but with errors. For more information, see troubleshooting documentation.
```

---

## Snippets

---

##### basic Buildfile

```
build: gradle assembly:assembly -DdescriptorId=jar-with-dependencies
```
[docs][3]

##### basic Procfile

```
web: java -jar path/to/my_app.jar
```
[docs][2]

##### basic .ebextensions/env.config

```yml
option_settings:
  aws:elasticbeanstalk:environment:proxy:staticfiles:
    /static: static
#  - namespace:  aws:elasticbeanstalk:application:environment
#    option_name: PORT
#    value: 6000
```
[docs][1]

---

## Examples 

---

##### Buildfile

```
build: gradle assembly:assembly -DdescriptorId=jar-with-dependencies
```

##### Procfile

```
web: java -jar build/libs/my_app.jar
```

---

##### DB credentials

JDBC uses a connection string in the following format:
`jdbc:,driver://hostname:port/schema?user=username&password=password`

```
spring.datasource.url=jdbc:mysql://${RDS_HOSTNAME}:${RDS_PORT}/${RDS_DB_NAME}?user=${RDS_USERNAME}&password=${RDS_PASSWORD}
```

---

# Circle CI

---

[1]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environment-cfg-staticfiles.html
[2]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-procfile.html
[3]: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-se-buildfile.html
[4]: https://medium.com/agorapulse-stories/how-to-deploy-java-application-jar-to-aws-beanstalk-with-gradle-35343337febf