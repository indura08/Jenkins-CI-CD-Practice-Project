pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                //mehma ''' daala thiynne me wage udrutha thunak athule apita one thrm sh command liynna puluwan
                sh '''          
                    ls -la
                    node --version
                    npm --version
                    npm install @angular/cli
                    npm ci
                    ./node_modules/.bin/ng build        
                '''
                // ng build eka paara ghnna bha mokda angular cliek globally install wela nathi hinda , enisa node_modules walt gihilla thami wenama ng build eka ganne meke aocmmand ek deela thiynwa wage 
                //The npm ci command is used to install Node.js project dependencies in a clean and reliable way. It is similar to npm install, but with a few important differences that make it particularly useful for CI (Continuous Integration) environments like Jenkins.
            }
        }

        stage('Test'){
            steps{
                sh 'Test -f dist/jenkinsproject/browser/index.html'
            }
        }
    }
}
