pipeline {
    agent any

    environment {
        BASE_VERSION = "1.0"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your repository
                checkout scm
            }
        }

        stage('Determine Branch') {
            steps {
                script {
                    // Adjust the branch variable name as required by your SCM plugin.
                    // Commonly, it might be env.BRANCH_NAME or env.GIT_BRANCH.
                    def branch = env.BRANCH_NAME ?: env.GIT_BRANCH

                    echo "Building branch: ${branch}"

                    // Set a flag to update version only on main branch.
                    if (branch == "origin/main") {
                        env.IS_MAIN = "true"
                    } else {
                        env.IS_MAIN = "false"
                    }
                }
            }
        }

        stage('Update Version on Main Branch') {
            when {
                expression { env.IS_MAIN == "true" }
            }
            steps {
                script {
                    // Construct a new version using BASE_VERSION and the Jenkins BUILD_NUMBER.
                    def newVersion = "${env.BASE_VERSION}.${env.BUILD_NUMBER}"
                    sh "git pull"
                    echo "Updating Maven project version to ${newVersion}"

                    // Update the pom.xml version using the Maven versions:set goal.
                    sh "/opt/homebrew/bin/mvn versions:set -DnewVersion=${newVersion} -DgenerateBackupPoms=false"

                    // Optionally commit the version change so next build starts with an updated version.
                    // Uncomment the following lines if your workflow includes committing changes back to Git.


                    sh "git config user.email 'dayananda30@gmail.com'"
                    sh "git config user.name 'dayananda30'"
                    sh "git commit -am 'Bump version to ${newVersion}'"
                    sh "git push origin main"
                }
            }
        }

        stage('Build and Test') {
            steps {
                // Clean, compile, package, and run tests
                sh "/opt/homebrew/bin/mvn clean install"
            }
        }
    }

    post {
        success {
            echo 'Build and tests passed!'
        }
        failure {
            echo 'The build or tests have failed.'
        }
    }
}