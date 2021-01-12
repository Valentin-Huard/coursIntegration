pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'JDK'
    }
    parameters {
        booleanParam(name: "Perform release ?", description: '', defaultValue: false)
    }
    stages {
        stage('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Deploy') {​​​​
            steps {​​​​
                sh "mvn -s /Users/valentinhuard/.m2/settings.xml deploy"
            }​​​​
        }​​​​
        stage('Release') {
            when { expression { params['Perform release ?']} }
                steps {
                    script{
                        pom = readMavenPom file: 'pom.xml'
                    }
                    withCredentials([usernamePassword(credentialsId: 'mjidsaa', passwordVariable: 'PASSWORD_VAR', usernameVariable: 'USERNAME_VAR')]){
                    sh 'git config --global user.email "you@example.com"'
                    sh 'git config --global user.name "Test"'
                    sh 'git branch release/'+pom.version.replace("-SNAPSHOT","")
                    sh 'git push origin release/'+pom.version.replace("-SNAPSHOT","")
                    sh 'mvn release:prepare -s /Users/valentinhuard/.m2/settings.xml -B -Dusername=$USERNAME_VAR -Dpassword=$PASSWORD_VAR'
                    sh 'mvn release:perform -s /Users/valentinhuard/.m2/settings.xml -B -Dusername=$USERNAME_VAR -Dpassword=$PASSWORD_VAR'
                }
            }

        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }}}

    