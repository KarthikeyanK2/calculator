 def resp = input id: 'ctns-prompt', message: 
  'Continue to the next stage?', 
  parameters: [string(defaultValue: '', description: '',
  name: 'para1')], submitterParameter: 'approver'
echo "Answered by " + resp['approver']
pipeline {
   agent any
   stages {
      stage("Checkout") {
          steps {
            git url: 'https://github.com/KarthikeyanK2/calculator.git'
            sh "chmod +x gradlew"
          }
      }
      stage("Compile") {
          steps {
              sh "./gradlew compileJava"
          }
      }
      stage("Unit test") {
          steps {
              sh "./gradlew test"
          }
      }
      stage("Code coverage") {
          steps {
              sh "./gradlew jacocoTestReport"
              publishHTML (target: [
                  reportDir: 'build/reports/jacoco/test/html',
                  reportFiles: 'index.html',
                  reportName: "JaCoCo Report"
              ])
              sh "./gradlew jacocoTestCoverageVerification"
          }
      }
      stage("Static code analysis") {
          steps {
              sh "./gradlew checkstyleMain"
              publishHTML (target: [
                  reportDir: 'build/reports/checkstyle/',
                  reportFiles: 'main.html',
                  reportName: "Checkstyle Report"
              ])
          }
      }
   }
}
