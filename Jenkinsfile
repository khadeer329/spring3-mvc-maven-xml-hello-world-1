pipeline {
    agent any 
    tools {
        // Note: this should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
        maven "Maven"
        
    }

    stages {
        stage("clone code") {
            steps {
                script {
                    // Let's clone the source
                    git 'https://github.com/khadeer329/spring3-mvc-maven-xml-hello-world-1.git';
                }
            }
        }
        stage("mvn build") {
            steps {
                script {
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    sh 'mvn -Dmaven.test.failure.ignore=true install'
                }
            }
        }
stage('publish to nexus') {
    steps {
        script {
            def pom = readMavenPom file: 'pom.xml'

            def groupId = pom.groupId
            def artifactId = pom.artifactId
            def version = pom.version
            def packaging = 'war'

            def filesByGlob = findFiles(glob: 'target/*.war')

            if (filesByGlob.length == 0) {
                error 'No WAR file found in target directory'
            }

            def artifactPath = filesByGlob[0].path

            echo "Artifact: ${artifactPath}"
            echo "Group ID: ${groupId}"
            echo "Artifact ID: ${artifactId}"
            echo "Version: ${version}"

            if (fileExists(artifactPath)) {
                nexusArtifactUploader(
                    nexusVersion: NEXUS_VERSION,
                    protocol: NEXUS_PROTOCOL,
                    nexusUrl: NEXUS_URL,
                    groupId: groupId,
                    version: version,
                    repository: NEXUS_REPOSITORY,
                    credentialsId: 'nexus-credentials',
                    artifacts: [
                        [
                            artifactId: artifactId,
                            classifier: '',
                            file: artifactPath,
                            type: packaging
                        ],
                        [
                            artifactId: artifactId,
                            classifier: '',
                            file: 'pom.xml',
                            type: 'pom'
                        ]
                    ]
                )

                echo 'Artifact uploaded to Nexus successfully'
            } else {
                error "Artifact not found: ${artifactPath}"
            }
        }
    }
}
    }
}
