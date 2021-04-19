pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    // environment {
    //     ArtifactId = readMavenPom().getArtifactId()
    //     Version = readMavenPom().getVersion()
    //     Name = readMavenPom().getName()
    // }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        //Stage3 : Publish the artifacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts:
                [[artifactId: 'EdDevOpsLab',
                  classifier: '',
                  file: 'target/EdDevOpsLab-0.0.4.war',
                  type: 'war']],
                credentialsId: '165739c5-9d00-4929-a22e-317f38338c1e',
                groupId: 'com.eddevopslab',
                nexusUrl: '172.20.10.88:8081',
                nexusVersion: 'nexus3',
                protocol: 'http',
                repository: 'EdDevOpsLab-RELEASE',
                version: '0.0.4'
            }
        }

        // Stage3 : Publish the artifacts to Nexus
        // stage ('Publish to Nexus'){
        //     steps {
        //         nexusArtifactUploader artifacts:
        //         [[artifactId: 'EdDevOpsLab',
        //         classifier: '',
        //         file: 'target/EdDevOpsLab-0.0.4-SNAPSHOT.war',
        //         type: 'war']],
        //         credentialsId: '165739c5-9d00-4929-a22e-317f38338c1e',
        //         groupId: 'com.eddevopslab',
        //         nexusUrl: '172.20.10.88:8081',
        //         nexusVersion: 'nexus3',
        //         protocol: 'http',
        //         repository: 'EdDevOpsLab-SNAPSHOT',
        //         version: '0.0.4-SNAPSHOT'
        //     }
        // }

        // Stage 4 : Print some information
//        stage ('Print Environment variables'){
//            steps {
//                echo "Artifact ID is '${ArtifactId}'"
//                echo "Version is '${Version}'"
//                echo "GroupID is '{}'"
//                echo "Name is '${Name}'"
//            }
//        }

        // Stage 5 : Deploying the build artifact to Apache Tomcat
        // stage ('Deploy to Tomcat'){
        //     steps {
        //         echo "Deploying ...."
        //         sshPublisher(publishers:
        //         [sshPublisherDesc(configName: 'Ansible Controller',
        //             transfers: [sshTransfer(cleanRemote: false,
        //             excludes: '',
        //             execCommand: '',
        //             execTimeout: 120000,
        //             flatten: false,
        //             makeEmptyDirs: false,
        //             noDefaultExcludes: false,
        //             patternSeparator: '[, ]+',
        //             remoteDirectory: '',
        //             remoteDirectorySDF: false,
        //             removePrefix: '',
        //             sourceFiles: '')],
        //             usePromotionTimestamp: false,
        //             useWorkspaceInPromotion: false,
        //             verbose: false
        //         )])
        //     }
        // }

        stage ('Deploy to Tomcat'){
            steps {
                echo "Deploying ...."
                sshPublisher(publishers:
                [sshPublisherDesc(
                    configName: 'Ansible Controller',
                    transfers: [
                        sshTransfer(
                                cleanRemote:false,
                                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_as_tomcat_user.yaml -i /opt/playbooks/hosts',
                                execTimeout: 120000
                        )
                    ],
                    usePromotionTimestamp: false,
                    useWorkspaceInPromotion: false,
                    verbose: false)
                    ])

            }
        }

    }
}