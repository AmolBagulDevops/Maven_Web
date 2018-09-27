node{
    def mvnHome
    stage('Checkout'){
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/AmolBagulDevops/Maven_Web.git']]])
    }
    stage('Build'){
        mvnHome = tool 'Maven'
        sh "'${mvnHome}'/bin/mvn clean install"
    }
    stage('Test'){
        mvnHome = tool 'Maven'
        sh "'${mvnHome}'/bin/mvn test"
    }
    stage('Result'){
        archiveArtifacts 'target/*.war'
    }
    stage('SonarQube Analysis'){
        mvnHome = tool 'Maven'
        withSonarQubeEnv('Sonar') {
            sh "'${mvnHome}'/bin/mvn sonar:sonar"
        }
    }
    stage('Publish to Nexus'){
        nexusPublisher nexusInstanceId: '12345', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/my-app-web.war']], mavenCoordinate: [artifactId: 'my-app-web', groupId: 'com.mycompany.app', packaging: 'war', version: '2.0']]]
    }   
     stage('Deploy Tomcat'){                
        sh 'sudo cp target/*.war /var/lib/tomcat8/webapps/'
    }
}
