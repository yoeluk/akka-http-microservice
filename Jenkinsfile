def sbtCmd = "sbt"

pipeline {
agent {
    label 'sbt'
}
stages {
    stage('Build App') {
    steps {
        git branch: 'master', url: 'https://github.com/yoeluk/akka-http-microservice'
        sh "${sbtCmd} assembly"
    }
    }
    stage('Build Image') {
    steps {
        script {
        openshift.withCluster() {
            openshift.withProject(env.DEV_PROJECT) {
            openshift.selector("bc", "sbt-example").startBuild("--from-file=target/scala-2.12/akka-http-microservice-assembly-1.0.jar", "--wait=true")
            }
        }
        }
    }
    }
    stage('Deploy') {
    steps {
        script {
        openshift.withCluster() {
            openshift.withProject() {
            openshift.selector("dc", "sbt-example").rollout().latest();
            }
        }
        }
    }
    }
}
}
