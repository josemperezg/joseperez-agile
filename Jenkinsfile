pipeline {
    agent any
    stages {
      stage('Test and Build') {
        parallel {
          stage('Run Tests') {
            steps {
              sh 'npm run test'
            }
          }
          stage('Create Build Artifacts') {
            steps {
              sh 'npm run build'
            }
          }
        }
      }
      stage('Deployment') {
        parallel {
          stage('Staging') {
            when {
              branch 'staging'
            }
            steps {
              withAWS(region:'<your-bucket-region>',credentials:'<AWS-Staging-Jenkins-Credential-ID>') {
                s3Delete(bucket: '<bucket-name>', path:'**/*')
                s3Upload(bucket: '<bucket-name>', workingDir:'build', includePathPattern:'**/*');
              }
              mail(subject: 'Staging Build', body: 'New Deployment to Staging', to: 'jenkins-mailing-list@mail.com')
            }
          }
        }
      }
    }
}
