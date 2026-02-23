pipeline {
    agent any

    // Trigger: every 5 minutes on Mondays
    triggers {
        cron('H/5 * * * 1')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the Spring PetClinic project...'
                sh 'mvn clean package'
            }
        }

        stage('Run Tests with JaCoCo') {
            steps {
                echo 'Generating JaCoCo coverage report...'
                sh 'mvn test jacoco:report'
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