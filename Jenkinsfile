node {
    stage('Clone sources') {
        git branch: 'main', url: 'https://github.com/mferen/angular-products-crud.git'
    }
    stage('Build & Push Image Backend') {
        withCredentials([usernamePassword(credentialsId: 'dockercred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh "docker login -u=$USERNAME -p=$PASSWORD"
        }

        dir("backend") {
            sh "docker build -t mferen03/mycrudbackendapp:1.0.0 ."
            //sh "docker push mferen03/mycrudbackendapp:1.0.0"
        }
    }
    stage('Build & Push Image Frontend') {
        withCredentials([usernamePassword(credentialsId: 'dockercred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh "docker login -u=$USERNAME -p=$PASSWORD"
        }
        dir("frontend") {
            sh "docker build -t mferen03/mycrudfrontendapp:1.0.0 --build-arg API_URL=http://$SERVER_IP:3001 ."
            sh "docker push mferen03/mycrudfrontendapp:1.0.0"
        }
    }
    stage('Run App') {
        withCredentials([sshUserPrivateKey(credentialsId: "sshcred", keyFileVariable: 'keyfile')]) {
            sh "ssh -oStrictHostKeyChecking=no -i ${keyfile} ubuntu@$SERVER_IP sudo docker compose -f /home/ubuntu/myappcrud/docker-compose-with-dockerhub.yaml down"
            sh 'scp -oStrictHostKeyChecking=no -i ${keyfile} docker-compose-with-dockerhub.yaml ubuntu@$SERVER_IP:/home/ubuntu/myappcrud'
            sh "ssh -oStrictHostKeyChecking=no -i ${keyfile} ubuntu@$SERVER_IP sudo docker compose -f /home/ubuntu/myappcrud/docker-compose-with-dockerhub.yaml pull"
            sh "ssh -oStrictHostKeyChecking=no -i ${keyfile} ubuntu@$SERVER_IP sudo docker compose -f /home/ubuntu/myappcrud/docker-compose-with-dockerhub.yaml up -d"
       }
    }
}
