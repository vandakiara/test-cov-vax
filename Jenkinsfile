pipeline {
  agent any

  tools {
    nodejs "node"
    }
  
  stages {
    stage('Install dependencies') {
      steps {
        bat 'npm install'
      }
    }
     
    stage('Test') {
      steps {
         bat 'npm run cypress:run'
      }
    }      
  }
  post {
    success {
      sendTelegram('Go schedule yourself the vaccine!')
    }
  }
}

def sendTelegram(message) {
    def encodedMessage = URLEncoder.encode(message, "UTF-8")

    withCredentials([string(credentialsId: 'telegramToken', variable: 'TOKEN'),
    string(credentialsId: 'telegramChatId', variable: 'CHAT_ID')]) {

        response = httpRequest (consoleLogResponseBody: true,
                contentType: 'APPLICATION_JSON',
                httpMode: 'GET',
                url: "https://api.telegram.org/bot$TOKEN/sendMessage?text=$encodedMessage&chat_id=$CHAT_ID&disable_web_page_preview=true",
                validResponseCodes: '200')
        return response
    }
}