node ('beaware-jenkins-slave') {
    stage('Download Latest') {
        git(url: 'https://github.com/beaware-project/social-media-analysis-mongo.git', branch: 'master')
        sh 'git submodule init'
        sh 'git submodule update'
    }

    stage ('Build docker image') {
        //sh 'mvn clean install'
        sh 'docker build -t beaware/social-media-analysis-mongo .'
    }

    stage ('Push docker image') {
        withDockerRegistry([credentialsId: 'dockerhub-credentials']) {
            sh 'docker push beaware/social-media-analysis-mongo'
        }
    }

    stage ('Deploy') {
        sh 'kubectl apply -f kubernetes/deploy.yaml -n prod --validate=false'
    }
    
    stage ('Print-deploy logs') {
        sh 'sleep 60'
        sh 'kubectl  -n prod logs deploy/social-media-analysis-mongo -c social-media-analysis-mongo'
    }    
}