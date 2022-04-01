pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/jfrog/project-examples.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "172.31.35.169",
                    credentialsId: AP6Ah9ENPXfvzToGmaX14pTq35J
                )
            }
        }

        stage ('Build docker image') {
            steps {
                script {
                    docker.build(yanivf-docker-sandbox-tlv + '/hello-world:latest', 'jenkins-examples/pipeline-examples/resources')
                }
            }
        }

        stage ('Push image to Artifactory') {
            steps {
                rtDockerPush(
                    serverId: "ARTIFACTORY_SERVER",
                    image: yanivf-docker-sandbox-tlv + '/hello-world:latest',
                    // Host:
                    // On OSX: "tcp://127.0.0.1:1234"
                    // On Linux can be omitted or null
                    host: 'tcp://localhost:2375',
                    targetRepo: 'yanivf-docker-sandbox-tlv',
                    // Attach custom properties to the published artifacts:
                    properties: 'project-name=docker1;status=stable'
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
    }
}
