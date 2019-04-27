properties([pipelineTriggers([githubPush()])]) 

node('linux'){
    stage('Unit Test'){
        git 'https://github.com/rclc/java-project.git'
        sh "ant"
        sh "ant -f test.xml -v"
        junit 'reports/result.xml'
    }
    
    stage('Build'){
        sh "ant -f build.xml -v"
    }
     stage('Deploy'){
          withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '886a4182-98c4-4bab-8549-cbe66191fb5c', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    // some block
        sh "aws s3 cp /workspace/java-pipeline/dist/rectangle-* s3://kehindek-assignment-4/"
       }
     }
    
    stage('Reports'){
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '886a4182-98c4-4bab-8549-cbe66191fb5c', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    // some block
        sh 'aws cloudformation describe-stack-resources --stack-name jenkins2 --region us-east-1' 
          }
        }
}
