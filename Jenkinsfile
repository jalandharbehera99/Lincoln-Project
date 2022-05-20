pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/jalandharbehera99/Lincoln-Project.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "http://13.127.189.46:8082",
                    credentialsId: "jfrogcreds",
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "http://13.127.189.46:8082",
                    releaseRepo: http://13.127.189.46:8081/artifactory/lincoln_artifacts,
                    snapshotRepo: http://13.127.189.46:8081/artifactory/lincoln_artifacts,
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "http://13.127.189.46:8082",
                    releaseRepo: http://13.127.189.46:8081/artifactory/lincoln_artifacts,
                    snapshotRepo: http://13.127.189.46:8081/artifactory/lincoln_artifacts,
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: MAVEN_TOOL, // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "13.127.189.46:8082"
                )
            }
        }
    }
}
