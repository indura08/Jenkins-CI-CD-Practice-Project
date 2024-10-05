pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '36c19340-1392-4120-accf-6175a16ca23e'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')     //api jenkins dashboard ekt ghilla menna me wage jenkins-token kiyla token ekk hduwa (secret text ekk), ann e jenkins wala hdapu tokens secret keys wage ewa thamimetha me credentials kiyla deenne , ehmai jenkins aduragnne credentaials
    }

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

        stage('Tests') {
            parallel {
                stage('Unit tests') {
                    agent {
                        docker {
                            image 'node-20-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh ''' 
                            #test -f dist/jenkinsproject/browser/index.html
                            npm install @angular/cli
                            npm ci
                            ./node_modules/.bin/ng test
                        '''
                    }
                    post {
                        always {
                            junit 'jest-result/junit.xml'
                        }
                    }
                }

                stage('E2E'){
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }
                    steps {
                        sh ''' 
                            npm install serve
                            ./node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test --reporter=html
                        '''
                    }

                    post {
                        always {
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
                    }
                }
        }

    }

        // stage('Test'){
        //     agent {
        //         docker {
        //             image 'node:20-alpine'
        //             reuseNode true
        //         }
        //     }
        //     steps{
        //         //me shell command ekn krnne dist folder eka athule jenkinsproject folder ek athule browser folder eka athule index.html file ek thiynwad kiyla balana ek , meka boho sarala test kirimak
        //         sh '''
        //                 test -f dist/jenkinsproject/browser/index.html
        //                 ng test
                        
        //             '''
        //     }
        // }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                //mehma ''' daala thiynne me wage udrutha thunak athule apita one thrm sh command liynna puluwan
                sh '''          
                    npm install netlify-cli   
                    node_modules/.bin/netlify --version  
                    echo "Deploying to production. Site Id : $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status 
                '''
                // ng build eka paara ghnna bha mokda angular cliek globally install wela nathi hinda , enisa node_modules walt gihilla thami wenama ng build eka ganne meke aocmmand ek deela thiynwa wage 
                //The npm ci command is used to install Node.js project dependencies in a clean and reliable way. It is similar to npm install, but with a few important differences that make it particularly useful for CI (Continuous Integration) environments like Jenkins.
            }
        }
    }

    //post kiynne me agents la tika run unain passe kiyna eki
    // post {
    //     //methna always kiynne pipeline ek fail unath success unath mekarun wenna kiyna eki
    //     always {
    //         junit 'test-results/junit.xml'
    //     }
    // }
}

//next day pipeline ek balala hariyt hdgnna 