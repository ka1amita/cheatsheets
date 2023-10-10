
## How-to

### How-to create necessary *AWS Resources*

1. **create** *VPC*
1. **create** *EC2*
1. **create** and **connect** *RDS*
1. **upload**, **configure**
1. **execute**

### How-to Create an EC2 

1. create *EC2 instance*
    *EC2* > *Lounch an Instance* > 
    + \> *Amazon Machine Image* 
        + *Amazon Machine Image (AMI)*: e.g. `Amazon Linux 2023 AMI`
        + *Architecture* :`64-bit (x86)`
    + ...
1. \> *Key Pair* (login)
    + add *Key Pair*
        *EC2* > *EC2 Dashboard* > *Resources* > *Key pairs* > *Acitons* > *Import key pair* > paste the output of `cat ~/.ssh/my_public_key.pub`[^public-key] inside the box    
    + create a special configuration
    ```
    Host my_server
    HostName x.y.z.w
    IdentityFile ~/.ssh/my_public_key.pub
    IdentitiesOnly yes
    User my_user
    Port
    ```
1. \> *Network settings*
    + Allow **SSH** traffic from ...
### How-to Create a RDS 

1. create *EC2 instance*
    *EC2* > *Create database* >
    + ...
    + \> *Templates*
        `Free tier`
    + ...
    + \> *Connectivity*
        + *Compute resource*: `Connect to an EC2 compute resource`!
        note the automatic set-up of *Security groups* etc.
    + \> *Additional configuration*
        + *Initial database name*: `my_schema`


### How-to upload and execute application

1. **login** with *Secure Shell*
see *EC2* > *Resources* >  *Instances* > my *Instance id* > *Actions* > *Connect* > **SSH client**
    + use `ssh instance_user@x.y.z.w` with configured `~/.ssh/config`
    + use `ssh -i path/to/my_key_pair.pem instance_user@in.stan.ce.ip` with a file
        + [Forum][2]

1. **install** *Java* with *yum* Package Manager
    1. `His $` `sudo yum update -y`
    1. `His $` `sudo yum install java-1.8.0-amazon-corretto` or `sudo yum install java-1.8.0-amazon-corretto-devel`
    1. `His $` `java -version`[^java-version]
    + [Docs][1]
    1. do whatever you want
1. **upload** with *Secure copy*
    + `Matej's $` `scp -i path/to/my_key_pair.pem path/to/my_code.jar instance_userin.stan.ce.ip:file/path`
1. meanwhile set *ENVs*
    +   from *.env*
        `His $` `export $(grep -v '^#' .env | xargs)`
        + [Set environment variables from .env file][3]
    +   manually
        `His $` `export My_ENV=my_value`       
1. **execute** the code
    1. `His $` `java -jar my_application.jar`
1. **connect**
    + to *Public IPv4 address* `my.app.ip.address:my_app_port` or *Public IPv4 DNS* `ec2-18-156-77-60.eu-central-1.compute.amazonaws.com`
 





### Examples

##### connect and upload
`EC2_DOMAIN=ec2-18-156-77-60.eu-central-1.compute.amazonaws.com`

...

`ssh -i ~/.ssh/aws-ec2-dev.pem ec2-user@$EC2_DOMAIN`

`scp build/libs/application.jar ec2-user@$EC2_DOMAIN:/home/ec2-user`


##### application environmental variables

`scp dev.env ec2-user@$EC2_DOMAIN:/home/ec2-user`

`dev.env`:
```properties
PORT=8080

RDS_HOSTNAME=project-db.ccpcqvumcguu.eu-central-1.rds.amazonaws.com
RDS_PORT=3306
RDS_DB_NAME=elastic_beanstalk
RDS_USERNAME=root
RDS_PASSWORD=password
```
`export $(grep -v '^#' dev.env | xargs)`

---

[^public-key]: the one set in `~/.ssh/config` under *IdentityFile* key

[^java-version]: should be:
    ```bash
    openjdk version "1.8.0_232"
    OpenJDK Runtime Environment Corretto-8.232.09.1 (build 1.8.0_232-b09)
    OpenJDK 64-Bit Server VM Corretto-8.232.09.1 (build 25.232-b09, mixed mode)
    ```

[1]: https://docs.aws.amazon.com/corretto/latest/corretto-8-ug/amazon-linux-install.html
[2]: https://superuser.com/questions/772660/howto-force-ssh-to-use-a-specific-private-key
[3]: https://stackoverflow.com/questions/19331497/set-environment-variables-from-file-of-key-value-pairs