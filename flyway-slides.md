# Flyway
---
## Integration
+ CLI
	+ `creating flyway.conf` in project root and using CLI
+ Dependency
    + `'org.flywaydb:flyway-core:8.5.12'`
    + `'org.flywaydb:flyway-mysql:8.5.12'`
    + adding to `application.properties` (note the extra `spring.` prefix) and using 
---
## Application Settings
---

### Basic set-up
```properties
#inside application.properties
#don't forget to change Hibernate settings
spring.jpa.hibernate.ddl-auto=validate

#if not defined, `spring.datasource.*` values used instead
spring.flyway.url=jdbc:mysql://localhost:3306
spring.flyway.schemas=my_schema
spring.flyway.user=my_root
spring.flyway.password=my_password

#optional (relative to `project_root/src/main/resources`; defaults to `/db/migration`)
#spring.flyway.locations=classpath:db/migration/{vendor}

#optional (defaults:)
#spring.flyway.sql-migration-prefix=V
#spring.flyway.repeatable-sql-migration-prefix=R
#spring.flyway.sql-migration-separator=__
#spring.flyway.sql-migration-suffixes=.sql
```
---
### Set-up in a non-empty DB
baseline when migrate is executed against a non-empty DB
```properties
#inside application.properties
spring.flyway.baseline-on-migrate=true
```
---
### Disable migration
```properties
#inside application.properties
spring.flyway.enabled=false
```
---
### Set placeholders
```properties
#inside application.properties
#optional (defaults:)
#spring.flyway.placeholder-replacement=true
#spring.flyway.placeholder-prefix=${
#spring.flyway.placeholder-suffix=}

#define placeholders
spring.flyway.placeholders.key=value
spring.flyway.placeholders.multi.layer.key=value
```
---
## Snippets
---
### Disable automatic migration on App start-up
(but still be able to trigger manually)

```java
    //inside Applicetion.class
    public static void main(String[] args) {
        SpringApplication.run(ProjectApplication.class, args);
    }

    @Bean
    FlywayMigrationStrategy flywayMigrationStrategy() {
        // prevents automatic migration on App start-up
        return args -> {
        };
    }
```
---
## Examples
---
### User with Placeholders
note the quotes
```sql
#inside V002__add_user.sql
insert into app_users (email, password, username,)
values ('${root.email}', '${root.password}', '${root.username}', now());
```
```properties
#inside application.properties
spring.flyway.placeholders.root.username=user_name
spring.flyway.placeholders.root.password=user_password
spring.flyway.placeholders.root.email=user_email
```
---
### Define script in java
```java
package db.migration;

@Component
public class V001__my_versioned_migration extends BaseJavaMigration {
    @Override
    public void migrate(Context context) throws Exception {
    try (PreparedStatement statement = context
            .getConnection()
            .prepareStatement("my SQL statement")) {
            statement.execute();

    	}
	}
}
```

---
### Use configuration file per script
use exactly the same name as for the the script, e.g. `V003__add_root.sql.conf`
```lisp
; inside .conf
shouldExecute=(${root.enable}==true) ; not supported by Flyway Community Edition!
```