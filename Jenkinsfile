pipeline {
    agent { node { label 'AGENT-1' } } 
    options{
        ansiColor('xterm')
    }
    // environment{
    //     packageVersion = ''
    // }
    stages {
        // push to featire branch
        // stage('Get Version'){
        //     steps{
        //         script{
        //             def packageJson = readJSON(file: 'package.json')
        //             packageVersion = packageJson.version
        //             echo "${packageVersion}"
        //         }
        //     }
        // }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Unit test'){
            steps{
                echo "unit test is done here"
            }
        }
        stage('sonar-scan'){
            steps{
                // sh 'ls -ltr'
                // sh 'sonar-scanner'
                echo "Sonar scan done"
            }
        }
        stage('Build'){
            steps{
                sh 'ls -ltr'
                sh 'zip -r catalogue.zip ./* --exclude=.git --exclude=.zip'
            }
        }
        stage('SAST'){
            steps{
                echo "SAST DOne"
                // echo "package version: $packageVersion"
            }
        }
        // install pipeline utility steps plugin
        stage('Pushing arificato to nexus'){
            steps {
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: '54.198.223.94:8081/',
                groupId: 'com.roboshop',
                version: "1.0.1",
                repository: 'catalogue',
                credentialsId: 'nexus-auth',
                artifacts: [
                    [artifactId: 'catalogue',
                    classifier: '',
                    file: 'catalogue.zip',
                    type: 'zip']
                ]
            )
                        }
        }
        // here i need to configure downstream job. i have to pass pkg version for deployment
        // this job will wait until downstream job is over
        // stage('Deploy'){
        //     steps{
        //         script{
        //             echo "Deployment"
        //             def params = [
        //                 string(name: 'version', value: "$packageVersion")
        //             ]
        //             // build job: "../catalogue-deploy", wait: true, parameters: params
        //         }
                
        //     }
        // }

    }
    // post{
    //     always{
    //         // need to delete workspace everytime
    //         echo "cleaning up workspace"
    //         //deleteDir()
    //     }
    // }
    post{
        always{
            echo 'cleaning up workspace'
            deleteDir()
        }
    }
}