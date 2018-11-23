#!groovy

def datas
def sprint

node {
    stage (' User Input') {
      def userInput = input(
      id: 'userInput', message: 'Let\'s promote?', parameters: [
      [$class: 'TextParameterDefinition', defaultValue: 'master', description: 'Sprint Tag', name: 'Sprint'],
      [$class: 'TextParameterDefinition', defaultValue: '', description: 'Comments', name: 'comments', isHidden: true]
      ])
     echo ("Input submited")
     sprint = userInput['Sprint'] 
     echo ("Sprint: ${sprint}")
    }      

   stage('Read File') {
        echo "Reading File"
        git branch: sprint, url: 'https://github.com/hpatak/pipeline.git'
        echo "Checked Out"
        datas = readYaml file: 'promotes2.yaml'
    }
}

for (microservice in datas) {
  echo "service name :" + microservice.name
  echo "service version:" + microservice.version          
  deployService (microservice.name,microservice.version)
}

def deployService(serviceName,serviceVersion) {
  node {
    echo "calling build ${serviceName}"
    stage(serviceName) {
        sh " oc start-build ${serviceName}-pipeline -F -n jenkins --env=IMAGE_TAG=${serviceVersion}"
    }
     echo "called build ${serviceName}"
  }
}
  