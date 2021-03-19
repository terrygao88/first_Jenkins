node {
    def commit_id
   stage('Preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }

    stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install --only=dev'
       sh 'npm test'
     }
   }

  stage('docker build/run/push') {
      def app = docker.build("terrygao8/my_repo:${commit_id}", '.')
      docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
      app.push()
        }   
     }
     
   stage('pull')
   {
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
        sh "docker pull terrygao8/my_repo:${commit_id}"
        sh "docker run -dp 3000:3000 terrygao8/my_repo:${commit_id}"
   }   
   }
   
}
