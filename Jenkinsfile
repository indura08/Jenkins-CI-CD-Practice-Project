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
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps{
                //me shell command ekn krnne dist folder eka athule jenkinsproject folder ek athule browser folder eka athule index.html file ek thiynwad kiyla balana ek , meka boho sarala test kirimak
                sh '''
                        test -f dist/jenkinsproject/browser/index.html
                        ng test
                        
                    '''
            }
        }
    }

    //post kiynne me agents la tika run unain passe kiyna eki
    post {
        //methna always kiynne pipeline ek fail unath success unath mekarun wenna kiyna eki
        always {
            junit 'test-results/junit.xml'
        }
    }
}
