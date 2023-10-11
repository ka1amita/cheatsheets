
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
1. \> *Key Pair* (**only** possible **now** during set-up!)
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
1. **exit**
    + `His $` `exit` **not** `logout` (works only locally)
1. **upload** with *Secure copy*
    + `Matej's $` `scp -i path/to/my_key_pair.pem path/to/my_code.jar instance_userin.stan.ce.ip:/file/path` the filepath behind `:` is oprional but not the `:`!
1. meanwhile set *ENVs* for (**only**) the *session*
    +   from *.env*
        `His $` `export $(grep -v '^#' .env | xargs)` will ignore comment lines
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
`ssh -i ~/.ssh/aws-ec2-dev.pem $EC2_USER@$EC2_DOMAIN`

+ to not require adding host adress to `~/.shh/known_hosts`
`ssh -o StrictHostKeychecking=no $EC2_USER@$EC2_DOMAIN -v`

+ to add to `~/.shh/known_hosts`
`ssh-keyscan -H $EC2_HOST >> ~/.ssh/known_hosts` or `echo "..." >> ~/.ssh/known_hosts`

`scp build/libs/application.jar ec2-user@$EC2_DOMAIN:~`

##### application environmental variables

`scp .env.$ENV ec2-user@$EC2_DOMAIN:~`

filepath `.env.$ENV`:
```properties
PORT=8080

RDS_HOSTNAME=project-db.ccpcqvumcguu.eu-central-1.rds.amazonaws.com
RDS_PORT=3306
RDS_DB_NAME=elastic_beanstalk
RDS_USERNAME=root
RDS_PASSWORD=password
```
`export $(grep -v '^#' .env.$ENV | xargs)`






## Snippets

##### set time-out

add to `~/.ssh/config` :
`ServerAliveInterval 60`


## Example

##### deploy over SSH with CircleCI

```yml
version: 2.1

jobs:
  build:
  #...
  deploy:
    machine:
      image: ubuntu-2204:2023.07.2
    steps:
      - add_ssh_keys:
          fingerprints:
            - "4f:a5:86:38:62:4b:4a:0e:42:2f:20:df:56:40:39:33"
      - run:
          name: Deploy Over SSH
          command: |
            ssh $EC2_USER@$EC2_DOMAIN "<remote deploy command>"

workflows:
  build-and-deploy:
    jobs:
      - build
      - deploy:
          requires:
            - build # only deploy once build job has completed
          filters:
            branches:
              only: main # only deploy on the main branch
```
#!/bin/bash -eo pipefail
`ls -la ~/.ssh`
    ```
    config
    id_rsa_4fa58638624b4a0e422f20df56403933 # kay fingerprint suffix
    ```
`cat ~/.ssh/config`
    ```
    Host *
        IdentitiesOnly no
        IdentityFile /home/circleci/.ssh/id_rsa_4fa58638624b4a0e422f20df56403933
    ```

`find build/libs -name '*.jar'` or `find . -path '**/build/libs/*.jar'`
note the `**` before the first directory!

spusteni pomoci *inline-scriptu* bez ktereho **zustane** client **pripojen** k host
```yml
ssh $SSH_USER@$SSH_HOST \>>EOF export $(grep -v "^#" .env.dev | xargs) && java -jar $(find . -name *.jar) EOF
```

##### no-hang-up

```sh
nohup java -jar *.jar & # note the `&`
```
+ `nohup:ignoring input and appending output to 'nohup.out'` is normal output
+ [stackoverflow][4]

##### execute java over ssh in circleCI pipeline and successfuly finish the job (exit)
+ with **script**
    ```sh
    ...
    ssh $SSH_USER@$SSH_HOST './run.sh' & sleep 15
    # ssh $SSH_USER@$SSH_HOST './run.sh' & # doesn't work!
    ```
    `./run.sh`:
    ```sh
    #!/bin/bash
    export $(grep -v "^#" .env.dev | xargs)
    nohup java -jar *.jar & 
    ```
    + don't forget to `chmod 744 run.sh` to add `x`
+ with **chained command**
    ```sh
    ssh $SSH_USER@$SSH_HOST 'export $(grep -v "^#" .env* | xargs) && nohup java -jar *.jar &' & sleep 15
    ```
    or
    ```sh
    ssh $SSH_USER@$SSH_HOST "export $(grep -v '^#' .env.$ENV | xargs) && nohup java -jar *.jar &" & sleep 15
    ```
    + note the `""` (`$ENV` has to be **locally** *expanded*)
+ with *\<<BLOCK*[^EOF] a.k.a *inline script* 
    ```sh
    ssh $SSH_USER@$SSH_HOST \<<EOF
        export $(grep -v "^#" .env.* | xargs)
        nohup java -jar *.jar &
        EOF & sleep 15
    ```
    
    + note is has to be **escaped** like `\<<` in *CircleCI* `config.yml`

[EOF:]`<< EoF` means:
> `<<` **read** the *multi-line input* that begins from the next line onward, and treat it as if it's **code in a separate file**
> `EoF` **stop reading** immediately after the word `EoF` is found in the *multi-line input*

---

## Misc

##### epanding the inputs
```sh
ssh $SSH_USER@$SSH_HOST java -jar $(find . -name *.jar) # find on local

ssh $SSH_USER@$SSH_HOST 'java -jar $(find . -name *.jar)' # find on host

ssh $SSH_USER@$SSH_HOST "java -jar $(find . -name *.jar)" # find on local

```

##### kill running *App* instance based on *port*
1. find out the *PID*
    +   `fuser 8080/tcp`
    +   `lsof -i :8080 -S`
    +   `netstat -nlp | grep :8080`
2.  `kill my_PID`

---

[^public-key]: the one set in `~/.ssh/config` under *IdentityFile* key

[^java-version]: should be:
    ```
    openjdk version "1.8.0_232"
    OpenJDK Runtime Environment Corretto-8.232.09.1 (build 1.8.0_232-b09)
    OpenJDK 64-Bit Server VM Corretto-8.232.09.1 (build 25.232-b09, mixed mode)
    ```

---

[1]: https://docs.aws.amazon.com/corretto/latest/corretto-8-ug/amazon-linux-install.html
[2]: https://superuser.com/questions/772660/howto-force-ssh-to-use-a-specific-private-key
[3]: https://stackoverflow.com/questions/19331497/set-environment-variables-from-file-of-key-value-pairs
[4]: https://stackoverflow.com/questions/24646320/nohupignoring-input-and-appending-output-to-nohup-out