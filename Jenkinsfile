pipeline {
    agent any

    triggers {
        cron('H/5 * * * 1')
    }

    tools {
        jdk 'Java25'
        maven 'Maven3'
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