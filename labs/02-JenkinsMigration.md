# Migrate your Jenkins Know How to Azure DevOps

## Jenkins Pipeline 

```groovy
//Defines the Build Pod with Name MavenBuildEnv
podTemplate(
    label: 'MavenBuildEnv', 
    cloud: 'kubernetes', 
    podRetention: never(),
    containers: [
        containerTemplate(
            name: 'maven',
            image: 'maven:3.3.9-jdk-8-alpine',
            command: 'cat',
            ttyEnabled: true,
            workingDir: '/home/jenkins',
        ), // End of first Container definition
    ], // End of Containers/Pods Definition
) // End of Pod Template 
{
    node('MavenBuildEnv'){
        stage('Checkout Source'){
            git 'http://git-endpoint.default.svc.cluster.local/albec/SpringFramework-Petclinic.git'
            echo "Done"
        }
        stage('Exec Tests'){
            container('maven'){
                sh 'mvn test'
                junit keepLongStdio: true, testResults: '**/TEST*.xml'
                archiveArtifacts artifacts: '**/TEST*.xml', onlyIfSuccessful: true
            }
        }
        stage('Build App'){
            container('maven'){
                sh 'mvn -B clean package -DskipTests '
            }
        }
        stage('Upload Artifacts'){
            archiveArtifacts artifacts: '**/*.war', onlyIfSuccessful: true
        }
    }
}
```

## Azure DevOps

```yaml
pool:
  name: Hosted Ubuntu 1604
  demands: maven

steps:
- task: Maven@3
  displayName: 'Maven pom.xml'
  inputs:
    options: '-DskipITs --settings ./maven/settings.xml'



- task: CopyFiles@2
  displayName: 'Copy Files'
  inputs:
    SourceFolder: '$(build.sourcesdirectory)'
    Contents: |
     **/target/*.war
     *.sql
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```



## Build vs. Release 

Difference in Architecture