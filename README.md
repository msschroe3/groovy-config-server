# Spring Cloud Config Server

This repo is a Spring Boot implementation of a [Spring Cloud Config](https://cloud.spring.io/spring-cloud-config) server backed by a Git repository.

### Property Source

Point this server at a GitHub repo. The repository should have an `application.yml` for shared properties and an individual file for each Spring Config Client. The name of the file should match the `spring.application.name` property in that service.

### Adding a Config Client

There are two main steps to adding a new client of the config server.

##### Add Properties to Repo Holding Config Files

1. Add file named `{spring.application.name}.yml` to repository holding all configuration
    - The spring.application.name property will be set in Step 2 of the next section
2. Add any common properties to `application.yml`

##### Update Service to be a Config Client

1. Create `bootstrap.yml` resource file (bootstrap gets loaded before `application.yml`, which
    will now be populated from the Config Server)
2. In `bootstrap.yml`, add these properties
    ```yml
    spring:
      application:
        name: some-service
      cloud:
        config:
          uri: {config-domain.com}/config
          fail-fast: true
    ```
3. Add the following dependencies to the `build.gradle` file to support Spring Cloud Config
    ```gradle
    dependencyManagement {
         imports {
              mavenBom 'org.springframework.cloud:spring-cloud-config:1.3.3.RELEASE'
         }
    }
    
    dependencies {
        compile 'org.springframework.cloud:spring-cloud-starter-config'
    }
    ```

### Other Notes

Update properties on the fly by adding `@RefreshScope` to a bean that wires in a 
property updated during runtime. To refresh beans with this scope,
you can POST to the service's `/refresh` endpoint.

See [this documentation](http://cloud.spring.io/spring-cloud-static/docs/1.0.x/spring-cloud.html#_refresh_scope)
for more details.
