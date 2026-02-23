pipeline {
    agent any

    // Trigger: every 5 minutes on Mondays
    triggers {
        cron('H/5 * * * 1')
    }

    tools {
        maven 'Maven3' // The name you set in Jenkins Global Tool Configuration
        jdk 'Java17'   // Make sure your Jenkins JDK is configured
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the Spring PetClinic project...'
                bat "${tool 'Maven3'}/bin/mvn clean package"
            }
        }

        stage('Run Tests with JaCoCo') {
            steps {
                echo 'Generating JaCoCo coverage report...'
                bat "${tool 'Maven3'}/bin/mvn test jacoco:report"
            }
        }

        stage('Publish Coverage') {
            steps {
                jacoco(
                    execPattern: 'target/jacoco.exec',
                    classPattern: 'target/classes',
                    sourcePattern: 'src/main/java'
                )
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}