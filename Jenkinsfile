pipeline {
    agent any
    parameters {
        string (
            defaultValue: 'fibonacci',
            description: 'The name of the project in CodeSonar',
            name : 'JOB_NAME'
        )
        string (
            defaultValue: '127.0.0.1:7340',
            description: 'The default address of the CodeSonar hub',
            name : 'HUB'
        )
    }

    stages {
        stage('build and analyze') {
            steps {
                sh "g++ fibonacci.cc"
                sh "codesonar analyze ${params.JOB_NAME} -name ${params.JOB_NAME} -foreground ${params.HUB} g++ fibonacci.cc"
                script {
                     codesonar conditions: [
                         redAlerts(warrantedResult: 'FAILURE'),
                         warningCountAbsoluteSpecifiedScoreAndHigher(rankOfWarnings: 0, warningCountThreshold: 0, warrantedResult: 'FAILURE'),
                         warningCountAbsoluteSpecifiedScoreAndHigher(rankOfWarnings: 0, warningCountThreshold: 2, warrantedResult: 'UNSTABLE'),
                     ],
                     credentialId: '5c96e7e9-9d4b-40ec-9bad-a77c48042e62', hubAddress: 'ec2-3-65-242-31.eu-central-1.compute.amazonaws.com:7340',
                     projectName: '${JOB_NAME}', protocol: 'http', visibilityFilter: '3'
                }
            }
        }
    }
    post {
        always {
            echo "Goodbye, World"
        }
        failure {
            echo "Hello, World"
        }
    }
}
