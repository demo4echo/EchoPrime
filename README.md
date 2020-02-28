# EchoPrime - Top Level Module

## The *demo4echo* account demonstrates an opinionated CI/CD solution powered by Jenkins declarative Pipeline and running over K8S 

### The modules/repositories of the project are:
- **echobe**: A µs representing a backend app (e.g. one that would connect to a data store)
- **echofe**: A µs representing a frontend app (e.g. one that would be exposed outside the K8S cluster)
- **EchoCommon**: The shared module of the project (“product”) with all Jenkins/Gradle shared logic
- **EchoPrime (this module)**: The project (“product”) level module controlling execution of all the µs
- **Echoe2eFunctionalCertification**: An E2E testing module (powered by Karate)
- **Echoe2ePerformanceCertification (future)**: An E2E load certification (to be powered by Predator)
- **JenkinsRemoteSlavePodDockerImage**: Pod builder image (Alpine/Ubuntu) - used by the Pipeline
- **JenkinsSharedLibrary**: Common Groovy variables and functions - used by the Pipeline
