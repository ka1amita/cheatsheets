#

## How-to

### How-to debug

[Docs on SSH access](https://circleci.com/docs/ssh-access-jobs/)

### How-to parse terraform outpu json

1. `jq -r 'to_entries|map("\(.key)=\(.value|tostring)")|.[]' environment_variables.dev`
1. `kalamita@Matejs-MacBook-Air backend % cat environment_variables.dev | jq '.["dns_address"]'`


### How-to add *keys* to *SSH Agent*

- Add the default ssh keys in `~/.ssh` to the ssh-agent:
    `ssh-add`

- Add a specific key to the ssh-agent:
    `ssh-add path/to/my_private_key`

### How-to add *keys* to a *remote* 

+ manually
`ssh -i "my_authorized.pem" user@host "echo $(cat ~/.ssh/my_unauthorized.pub) >> ~/.ssh/authorized_keys"`

+ `ssh-copy-id`
    + Copy **all** your *public keys* to the *remote* (use `-n` to do a dry-run):
        `ssh-copy-id user@host`
    + Copy the **given** *public key* to the *remote*:
        `ssh-copy-id -i path/to/my_certificate usern@host`


### How-to use environmental variables

> *CircleCI* **does not** support **interpolating** *environment variables* in the *configuration*!
> Values used are **treated** as *literals*. This can cause issues when defining `working_directory`, modifying `PATH`, and **sharing** variables **across** multiple **run steps**.

> *Vars* **declared** inside a *shell* command `run` *step* will **override** *vars* declared with the environment and *contexts* keys! Contexts *vars* will take precedence over *vars* **added** on the *Project Settings* page!

+ Both `${VAR}` and `$VAR` are supported.

+ *Secrets masking* is **applied** to *vars* set within *Project Settings* or *Contexts* in the *web app* **only**. 

[Docs on env. variables][5]
[Docs on built in env. variables][6]

#### How-to share values across steps!

> While *CircleCI* does **not support** *interpolation* when setting *environment variables* (*vars*), it is possible to set variables for the **current** *shell* by using `BASH_ENV`. This is useful for both **modifying** your `PATH` and *setting environment* variables that **reference other variables**.

> Depending on your *shell*, you may have to **append** the new *var* to a *shell* *startup file* like `~/.tcshrc` or `~/.zshrc` (or even `~/.bashrc`?).

> On *Linux*, the default *shell* is not a *login shell* (`--login` or `-l` are not specified). Hence, the shell will **not source** your `~/.bash_profile`, `~/.bash_login`, `~/.profile` files.

> For jobs that run on *macOS*, the shell is a non-interactive login shell. The shell will execute /etc/profile/ followed by ~/.bash_profile before every step.

> *CircleCI* uses *bash* to source `BASH_ENV` in **every** *step*. Allowing you to use *interpolation* and share *vars* **across run** *steps*[^1].

[^1]: The `$BASH_ENV` workaround only **works** with *bash*, and has **not** been **confirmed** to work with **other** *shells*.

> *Vars* in a *container* in this way are not available to steps run within the *container*, they are only **available** to the *entrypoint/command* run by the **container**. 

```yml
version: 2.1
- run:
    name: Update PATH and Define Environment Variable at Runtime
    # Add source command to execute code and make variables available in current step.
    command: |
      echo 'export PATH=/path/to/foo/bin:"$PATH"' >> "$BASH_ENV" # or ~/.shrc
      echo "export VERY_IMPORTANT=VALUE_CONTENT" >> "${BASH_ENV}" # or ~/.shrc
      source "$BASH_ENV"
```
[Docs on reusable code][7]

##### How-to substitute *vars*

> The `circleci env subst` command can accept text input from `stdin` or as an *argument*.

`template.file` e.g. as *json*:
```json
{
  "foo": "$FOO",
  "provider": "${PROVIDER}"
}
```

```yml
- run:
    name: Process template file
    environment:
      # Environment variables would typically be served via a context
      # FOO: bar
      # PROVIDER: circleci
    command: circleci env subst < template.file > output.file

- run: circleci env subst "hello \$WORLD" > output.json
```
The `<` symbol is used to **redirect** the contents of the `template.file` **file** as *input* to the `env subst` command, while the `>` symbol is used to **redirect** the *output* of the `env subst` command to the `output.file`.

#### How-to share *vars* with host

```yml
- run:
    name: Share VAR with host
    command: ssh $SSH_USER@$SSH_HOST "echo VAR=$LOCAL_VAR >> ~/.bashrc" # `"` necessary!
```
```yml
- run:
    name: Read VAR at host
    command: ssh $SSH_USER@$SSH_HOST 'echo $LOCAL_VAR' # `'` necessary!
# output: local_value
```

### How-to manage SSH keys

##### convert ppk (puTTy) key into OpenSSH's private key format:

`puttygen mykey.ppk -O private-openssh -o my-openssh-key` (without extension)
`puttygen mykey.ppk -O public-openssh -o my-openssh-key.pub`

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
    + export **to** host from **local**'s `.env.local_env`[^41] with `exec` and store the *PID* (**inherits** from the *shell*):
    ```sh
    ssh $SSH_USER@$SSH_HOST "export $(grep -v ^# .env.$ENV | xargs) && echo \${$} > app.pid && exec java -jar *.jar" & sleep 15`
    ```
    + export **on** host from **host**'s `.env.local_env`[^40] with `nohup` and `&`:
    ```sh
    #ssh $SSH_USER@$SSH_HOST "export \$(grep -v '^#' .env.$ENV | xargs) && nohup #java -jar *.jar &" & sleep 15
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

### How-to find a proccess and check it's running
+ List ALL running *processes*
    + `ps aux`
+ List all running *processes* that have a given *file* **open**:
    + `lsof $(find path/to/file -type f -name "*.jar")`
+ List all running *processes* **with** a *PID*
    + `cat my_app.pid | xargs ps`
+ List all running *processes* **on** *port*
    + `lsof -i :MY_PORT -S`
    + `lsof -i :MY_PORT -S`
    + `netstat -nlp | grep :MY_PORT`

##### kill running *App* instance based on *port*

1. find out the *PID*
1. `kill my_PID`

### How-to kill process / restart 
+ Restart machine
    + `reboot`   
+ Terminate using the default `SIGTERM` (*terminate*) signal
    + `killall -v java || true` 
        + `|| true` appended to avoid `exit 1` if there is no such process; there is another option to force *CircleCI* to run after *failed job*
+ Terminate a program based on *PID* using the default `SIGTERM` (*terminate*) signal:
    + `kill process_id`
---

## Misc



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
[5]: https://circleci.com/docs/env-vars
[6]: https://circleci.com/docs/variables/#built-in-environment-variables
[7]: https://circleci.com/docs/reusing-config/#using-the-parameters-declaration

# Dodatek

## Exercises

### Value Interpolation / Expansion

[Stackoverflow on value interpolation / expansion](https://stackoverflow.com/questions/6697753/difference-between-single-and-double-quotes-in-bash)

##### eppanding an inputs
```sh
ssh $SSH_USER@$SSH_HOST java -jar $(find . -name *.jar) # find on local

ssh $SSH_USER@$SSH_HOST 'java -jar $(find . -name *.jar)' # find on host

ssh $SSH_USER@$SSH_HOST "java -jar $(find . -name *.jar)" # find on local
```

##### export *vars* **on** *host* through *ssh*

1. `ssh $SSH_USER@$SSH_HOST "export \$(grep -v ^# .env.$ENV | xargs)"`[^40]
1. `ssh $SSH_USER@$SSH_HOST "export $(grep -v ^# .env.$ENV | xargs)"`[^41]
1. `ssh $SSH_USER@$SSH_HOST 'export $(grep -v ^# .env.$ENV | xargs).`[^42]

[^40]: export **on** host from **host**'s `.env.local_env`
[^41]: export **to** host from **local**'s `.env.local_env`
[^42]: export **on** host from **host**'s `.env.host_env` (if `ENV` present)

#####

1. `echo 'TEST=$USER' > test.user`
   `ssh $EC2_USER@$EC2_HOST "echo $(cat test.user) > test.user"`[^50] 
1. `echo TEST=$USER > test.user`
   `ssh $EC2_USER@$EC2_HOST "echo $(cat test.user) > test.user`[^51]

[^50]: `test.user`:
    ```
    TEST=$USER
    ```
    host's `USER` because only the `cat` command was run on local
[^51]: `test.user`:
    ```
    TEST=ka1amita
    ```

#####

1. `ssh $EC2_USER@$EC2_HOST "echo export TEST=$USER >> ~/.bashrc"`[^52]
1. `ssh $EC2_USER@$EC2_HOST "echo 'export TEST=$USER' >> ~/.bashrc"`[^53]
1. ```ssh $EC2_USER@$EC2_HOST "echo `export TEST=$USER` >> ~/.bashrc"```[^54]
1. `ssh $EC2_USER@$EC2_HOST echo export TEST=$USER >> ~/.bashrc`[^55]
1. `ssh $EC2_USER@$EC2_HOST 'echo export TEST=$USER' >> ~/.bashrc`[^56]
1. `ssh $EC2_USER@$EC2_HOST 'echo export TEST=$USER >> ~/.bashrc'`[^57]
[^52]: see [^53]
[^53]: `host:~/.bashrc:`
        ```
        export TEST=kalamita
        ```
        `'` has no special meaning inside `""`, but `echo "'$USER'"` results in `'user'`!
[^54]:  `host:~/.bashrc`
        ```
        (only a `newline`!)
        ```
[^55]: `local:~/.bashrc`
        ```
        export TEST=local
        ```
[^56]: `local:~/.bashrc`
        ```
        export TEST=host
        ```
[^57]: `host:~/.bashrc`
        ```
        export TEST=host
        ```

##### ssh with a command and it's arguments

`ssh $EC2_USER@$EC2_HOST cat ~/.bashrc`[^60]
[^60]: `~` interpolates on *local*, runs on *host*!

`ssh $EC2_USER@$EC2_HOST mv test.ka1amita test.kalamita`[^61]
[^61]: `mv` runs on *host*

`ssh $EC2_USER@$EC2_HOST mv test.ka1amita test.kalamita &`[^59]
[^59]: command runs on + *host* and in the backgroud from the point of *local*!

##### `ssh` with `cat`

`host:test.local`:
```
Correct!
```
1. `ssh $EC2_USER@$EC2_HOST cat test.$USER`[^62]
1. `ssh $EC2_USER@$EC2_HOST "cat test.$USER"`[^63]
1. `ssh $EC2_USER@$EC2_HOST 'cat test.$USER'`[^64]
1. `ssh $EC2_USER@$EC2_HOST cat $(echo test.$USER)`[^65]
1. `ssh $EC2_USER@$EC2_HOST "cat 'test.$USER'"`[^66]

[^62]: Correct!
[^63]: Correct!
[^64]: cat: test.ec2-user: No such file or directory
[^65]: Correct!
[^66]: Correct!

##### `ssh` with `echo` of `grep`
`
`host:test.local`:
```
Correct!
#Incorrect
```
1. `ssh $EC2_USER@$EC2_HOST $(echo grep -v ^# test.$USER)`[^67]
1. ```ssh $EC2_USER@$EC2_HOST `echo grep -v ^# test.$USER` ```[^68]
1. `ssh $EC2_USER@$EC2_HOST "echo grep -v ^# test.$USER"`[^69]

[^67]: Correct!
[^68]: Correct!
[^69]: *`grep -v ^# test.kalamita`*

### How-to continue job after a failed steps

+ Using logical *OR* `||` iside a shell command
    `command --fail || true` # always results in `exit 0`

+ **pipefail**
    
    + The whole step does **not fail** on `exit` != `0`
        `set +e` # override shell configuratio or add a *run* step to a command
    ```yml
    - run:
      command: |
        echo Running test
        set +e
        mkdir -p /tmp/test-results
        make test
    ```

    + `when` under `job`
        + dafault `on_success`
        + `always` for e.g. **upload** *logs* or *code-coverage *data somewhere.
        + `on_fail` for e.g. **store** some diagnostic data to help debug test failures, or to *run custom notifications*

    + `when` under `step`
    ...

    + `background` execution will **immediately** **proceed** to the next step

    + **`-e`**
    > Exit **immediatly** if a *pipeline* (which may consist of a single simple command), a subshel command enclosed in parentheses, or **one** of the commands executed as part of a command list enclosed by braces exits with a **non-zero status**.
    
    override shell configuratio or add `set +e` *run* step to a command

    + **`-o pipefail`**

    > If `pipefail` is enabled, the pipeline’s *return status* is the value of the **last** (**rightmost**) command to `exit` with a **non-zero status**, or zero if all commands exit successfully. The shell **waits** for **all** commands in the pipeline to terminate **before** **returning** a value.

    override shell configuratio or add `set +o pipefail` *run* step to a command

    > For jobs that run on ***Linux***, the default value of the *shell* option is **`/bin/ bash -eo pipefail`** if `/bin/bash` is present in the build container. Otherwise it  is **`/bin/sh -eo pipefail`**.

    > For jobs that run on ***macOS***, the default shell is **`/bin/bash --login -eo pipefail`**. The shell is a non-interactive *login shell*. The shell will **execute** `/etc/profile/` followed by `~/.bash_profile` before every step.

    [Docs](https://circleci.com/docs/configuration-reference/#default-shell-options)

