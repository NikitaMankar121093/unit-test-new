pipeline
    {
        agent any
stages{
        stage('clean workspace')
            {
                steps  
                {
                    cleanWs()
                    sh 'cd /var/lib/jenkins/workspace/*'
                }
            }
        stage('Cloning')
            {
                steps
                {  
                    echo "Clone started"
                    // Clone the repository using Git
                    sh 'cd /var/lib/jenkins/workspace/ && git clone https://github.com/shredhanbhar/unit-test.git'
                    sh 'rm -rf /var/lib/jenkins/workspace/unit-test/lib'
                    sh 'mkdir /var/lib/jenkins/workspace/unit-test/lib'
                    sh 'cd /var/lib/jenkins/workspace/unit-test/lib/ && git clone https://github.com/google/googletest.git'
                    echo "Clone done"
                }
            }

        stage('Build code')

            {
                steps
                {   
                    // Run the build command for your C/C++ project
                    echo "Build started"
                   // sh 'mkdir -r /var/lib/jenkins/workspace/unit-test/build'
                    sh 'cd /var/lib/jenkins/workspace/unit-test/build/ && cmake .. && make'
                    echo "Build ended"
                }
                     post{
                             always{
                             mail to: "nikita.mankar@bluebinaries.com",
                             subject: "Build Success",
                             body: "${BUILD_NUMBER}_Passed!"
                             }
                         }

            }
                 

        stage('Unit test')
            {
                steps
                {
                    // Run unit tests for your project
                    sh 'cd /var/lib/jenkins/workspace/unit-test/build/ && make test'
                }
                
            }
    
        stage('TeastNg Report')
            {
                steps   
                {
                    sh 'testNG()'
                }
            }
        
        stage('Build tar')
            {
                steps
                {
                    //building tar directory 
                    sh 'cd /var/lib/jenkins/workspace/unit-test/ && tar -czvf /var/lib/jenkins/workspace/unit-test/build.tar.gz /var/lib/jenkins/workspace/unit-test/build/'
                    echo "tar directory generated"
                }
            }    

        stage('Upload Artifacts to Nexus repo')
            {
                steps
                {
                    nexusArtifactUploader artifacts: [[artifactId: '${BUILD_NUMBER}', classifier: '', file: '/var/lib/jenkins/workspace/unit-test/build.tar.gz', type: 'tar']], credentialsId: '14e82302-00d5-404d-bdf1-0e861e0b8fdf', groupId: 'repo', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'unit-test', version: '03'
                }
                post{
                         always{
                         mail to: "nikita.mankar@bluebinaries.com",
                         subject: "Artifacts Uploaded",
                         body: "${BUILD_NUMBER}_Passed! Uploaded Artifacts to Nexus repo successfully"
                         }
                   }
            
            }

        stage('Doxygen')
            {
                steps
                {
                    sh 'cd /var/lib/jenkins/workspace/unit-test/build/ && make docs'
                    sh 'chmod -R 777 /var/lib/jenkins/workspace/unit-test/*'        
                    echo "index.html created"         
                }
            }
      }
                post{
                        failure{
                        mail to: "nikita.mankar@bluebinaries.com",
                        subject: "Failure",
                        body: "${BUILD_NUMBER}_FAIL!"
                            }
                    }
    }

