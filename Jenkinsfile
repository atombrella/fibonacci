pipeline {
    agent any
    parameters {
        string (
            defaultValue: 'fibonacci',
            description: 'The name of the project in CodeSonar',
            name : 'JOB_NAME',
        )
        booleanParam (
            defaultValue: '127.0.0.1:7340',
            description: 'The default address of the CodeSonar hub',
            name : 'HUB'
        )
    }
  
    stages {
        stage('build and analyze') {
            steps {
                sh "g++ fibonacci.cc"
                sh "codesonar analyze ${JOB_NAME} -name ${JOB_NAME} -foreground ${HUB} g++ fibonacci.cc"
            }
            steps {
                status = absoluteWarningCount(56, 2, false)
                if (status === "MARK-AS-UNSTABLE") {
                    // or sent out emails
                    currentBuild.result = "UNSTABLE"
                }
            }
        }
    }
}
