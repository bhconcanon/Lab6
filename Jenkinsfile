@Library("startendtime") _
properties([pipelineTriggers([eventTrigger(jmespathQuery("branch=='main"))])])
podTemplate(containers: [
    containerTemplate(name: 'maven', image: 'maven:3.8.1-jdk-11', command: 'sleep', args: '99d')
]) {
    node(POD_LABEL) {
        stage('SCM Checkout') {
            git url: 'https://github.com/bhconcanon/Lab5', branch: 'main'
        }
        stage('Start time') {
            container('maven') {
                stage('buildStart Time Stage') {
                    buildStart()
                }
            }
        }
        stage('Build') {
            container('maven') {
                git 'https://github.com/dylanmeh/Lab2_Java-App.git'
                stage('build') {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }        
        stage('Test') {
            container('maven') {
                stage('test') {
                    sh 'mvn test'
                }
            }
        }
        stage('JUnit') {
            container('maven') {
                stage('junit test') {
                    if (getTriggerCauseEvent.getTriggerCauseEvent() == 'true') {
                        junit 'target/surefire-reports/*.xml'
                    }
                }
            }
        }
        stage('Deploy') {
            container('maven') {
                stage('deploy') {
                    sh './scripts/deliver.sh'
                }           
            }
        }