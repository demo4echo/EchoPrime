# EchoPrime - Top Level Module

## The modules/repositories of the project are:
- **echobe**: A µs representing a backend app (e.g. one that would connect to a data store)
- **echofe**: A µs representing a frontend app (e.g. one that would be exposed outside the K8S cluster)
- **EchoCommon**: The shared module of the project (“product”) with all Jenkins/Gradle shared logic
- **EchoPrime (this module)**: The project (“product”) level module controlling execution of all the µs
- **Echoe2eFunctionalCertification**: An E2E testing module (powered by Karate)
- **Echoe2ePerformanceCertification (future)**: An E2E load certification (to be powered by Predator)
- **JenkinsRemoteSlavePodDockerImage**: Pod builder image (Alpine/Ubuntu) - used by the Pipeline
- **JenkinsSharedLibrary**: Common Groovy variables and functions - used by the Pipeline

## TL;DR:

The module deals with the product (in the sense of encapsulating all micro-services) logic itself. Currently is handles a top level Jenkins Pipeline that is able to trigger a parallel build (via Pipeline as well) of its subordinates micro-services.
