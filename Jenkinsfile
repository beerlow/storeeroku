#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    gitlabCommitStatus('build') {
        stage('check java') {
            sh "java -version"
        }

        stage('clean') {
            sh "chmod +x mvnw"
            sh "./mvnw clean"
        }

        stage('install tools') {
            sh "./mvnw com.github.eirslett:frontend-maven-plugin:install-node-and-yarn -DnodeVersion=v6.11.1 -DyarnVersion=v0.27.5"
        }

        stage('yarn install') {
            sh "./mvnw com.github.eirslett:frontend-maven-plugin:yarn"
        }

        stage('backend tests') {
            try {
                sh "./mvnw test"
            } catch(err) {
                throw err
            } finally {
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }

        stage('frontend tests') {
            try {
                sh "./mvnw com.github.eirslett:frontend-maven-plugin:gulp -Dfrontend.gulp.arguments=test"
            } catch(err) {
                throw err
            } finally {
                junit '**/target/test-results/karma/TESTS-*.xml'
            }
        }

 //       stage('package and deploy') {
 //           sh "./mvnw com.heroku.sdk:heroku-maven-plugin:1.1.1:deploy -DskipTests -Pprod -Dheroku.appName="
  //          archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
  //      }

       // stage('quality analysis') {
     //       withSonarQubeEnv('Sonar') {
     //           sh "./mvnw sonar:sonar"
      //      }
      //  }

   //     def dockerImage
    //    stage('build docker') {
     //       sh "cp -R src/main/docker target/"
      //      sh "cp target/*.war target/docker/"
       //     dockerImage = docker.build('storeeroku', 'target/docker')
      //  }

       // stage('publish docker') {
       //     docker.withRegistry('https://registry.hub.docker.com', 'docker-login') {
      //          dockerImage.push 'latest'
      //      }
      //  }
    }
}
