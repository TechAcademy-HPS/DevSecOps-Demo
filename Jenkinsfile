@Library("Shared-Libraries") _
pipeline{
    agent any
    
    
    stages{
      stage('Sample_Stage'){
          steps{
              script{
                  welcome.sample('Welcome to demo on Jenkins Shared-Libraries')
              }
          }
      }

     
    //   stage('GIT_Checkout'){
    //       steps {
    //           git 'https://github.com/krishnaprasadkeerthi/Ansible-Demo.git'
    //       }
    //   }

      stage('S_GIT_Checkout') {
        steps {
          script{
                    fetchCode.GitFetch()
                }
              }   
            }


    //   stage('Maven_Build'){
    //      steps {
    //          sh 'mvn clean install'
    //      }
    //   }

      stage('S_MAven'){
          steps{
              script{
                  maven.mavenClean()
                  maven.mavenTest()
                  maven.mavenCleanInstall()
                  }
               }
            }


    //     stage('ExecuteSonarQubeReport'){
    //       steps{
    //           withSonarQubeEnv('sonarqube8.9'){
    //               sh 'mvn sonar:sonar \
    //               -Dsonar.projectKey=A1	 \
			 //     -Dsonar.projectName=Spring-Boot-Thymeleaf \
			 //     -Dsonar.host.url=http://3.111.150.28:9000 \
			 //     -Dsonar.login=sqp_def8ec16daecd8ae2a1ade880d55cfcc97c5c530' 
    //              }
    //         }
    //     }

       stage(S_Sonar_Report){
           steps{
               withSonarQubeEnv('sonarqube8.9'){
                   script{
                       sonarQube.sonarAnalysis('A1','Spring-Boot-Thymeleaf','http://3.111.150.28:9000','sqp_def8ec16daecd8ae2a1ade880d55cfcc97c5c530')
                   }
               }
           }
       }

    //   stage(S_Quality_gates){
    //       steps{
    //           waitForQualityGate abortPipeline: true
    //       }
    //   }


        // stage('NexusArtifactUploader'){
        //     steps{
        //         nexusArtifactUploader artifacts: [
        //             [
        //              artifactId: 'spring-boot-thymeleaf',
        //              classifier: '',
        //              file: '/var/lib/jenkins/.m2/repository/pl/codeleak/demos/sbt/spring-boot-thymeleaf/2.0.0/spring-boot-thymeleaf-2.0.0.war',
        //              type: 'war'
        //              ]
        //         ],
        //         credentialsId: 'nexus', 
        //         groupId: 'pl.codeleak.demos.sbt', 
        //         nexusUrl: '13.233.100.229:8081', 
        //         nexusVersion: 'nexus3', 
        //         protocol: 'http', 
        //         repository: 'ansible', 
        //         version: '2.0.0'
        //     }
        // }     
     
        stage('S_NexusArtifactUploader'){
            steps{
                script {
                      nexus.nexus(
                      'spring-boot-thymeleaf',
                      '',
                      '/var/lib/jenkins/.m2/repository/pl/codeleak/demos/sbt/spring-boot-thymeleaf/2.0.0/spring-boot-thymeleaf-2.0.0.war',
                      'war',
                      'nexus',
                      'pl.codeleak.demos.sbt',
                      '13.233.100.229:8081',
                      'nexus3',
                      'http',
                      'ansible',
                      '2.0.0')
                }
            }
        }
     
        stage('Ansible'){
            agent{
                label 'ansible_agent'
            }
            steps{
		  script {
		        ansible.ansible_deploy_playbook('Inventory','playbook.yml')
		  }             		    
//  		    sh 'ansible-playbook -i Inventory playbook.yml '
            }
        }
        
    }
}
