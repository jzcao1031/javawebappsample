node {
  withEnv(['AZURE_SUBSCRIPTION_ID=1f142d4e-afa7-4f74-b4f6-0e5529fa53ec',
        'AZURE_TENANT_ID=191e8c8b-dd2a-4494-9b11-d922985e4d26']) {
    stage('init') {
      checkout scm
    }

    stage('build') {
      sh 'mvn clean package'
    }

    stage('deploy') {
      def resourceGroup = 'jenkins-get-started-rg'
      def webAppName = 'junzhe-calc-7391'
      // login Azure
      withCredentials([usernamePassword(credentialsId: 'AzureServicePrincipal', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
        sh '''
          az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
          az account set -s $AZURE_SUBSCRIPTION_ID
        '''
      }
      // upload package
      sh "az webapp deploy --resource-group $resourceGroup --name $webAppName --src-path target/calculator-1.0.war --type war"
      // log out
      sh 'az logout'
    }
  }
}
