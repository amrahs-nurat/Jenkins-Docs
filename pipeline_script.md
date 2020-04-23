#### How to run aws cli command in server:

1. Install without docker installation:
```
node {
  stage("List S3 buckets") {
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'mykeys', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    sh 'aws s3 ls'
    sh 'aws configure list'
		}
    }
  }
```
2. With Docker installation: ( Must install docker server, and docker plugin in jenkins)
```
@Library('github.com/releaseworks/jenkinslib') _

node {
  stage("List S3 buckets") {
    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws-key', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        AWS("--region=eu-west-1 s3 ls")
    }
  }
}
```

#### How to install package into python environment: 

```
node {
        stage ('script run') {
        sh "sudo apt-get install python3-pip -y"
        sh "sudo python3 -m pip --version"
        sh "sudo python3 -m pip install virtualenv"
        sh "virtualenv -p python3 /var/lib/jenkins/myenv"
        sh ". myenv/bin/activate"
        sh "/var/lib/jenkins/myenv/bin/python3 -m pip install cfn-lint"
        sh "/var/lib/jenkins/myenv/bin/pip3 list"
      }
    
}
```
Note: jenkins ALL=(ALL:ALL) NOPASSWD: ALL
