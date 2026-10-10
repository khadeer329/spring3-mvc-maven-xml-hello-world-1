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
            def groupId = 'com.ncodeit'
            def artifactId = 'ncodeit-hello-world'
            def version = '3.0'
            def packaging = 'war'

            def artifactPath = "target/${artifactId}-${version}.war"

            if (!fileExists(artifactPath)) {
                error "WAR file not found: ${artifactPath}"
            }

            echo "Uploading artifact: ${artifactPath}"
            echo "Group ID: ${groupId}"
            echo "Artifact ID: ${artifactId}"
            echo "Version: ${version}"

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
        }
    }
}
    }
}
