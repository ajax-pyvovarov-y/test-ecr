import groovy.transform.Field

@Field
def AWS_CREDENTIALS = [
    $class       : 'AmazonWebServicesCredentialsBinding',
    credentialsId: 'GLOBAL_M11G_INFRASTRUCTURE_ECR'
]


timeout(time: 60, unit: 'MINUTES') {
    timestamps {
        ansiColor('xterm') {
            node('ci-x86-enc-u22.04-m11g-v2-slave') {
                withEnv(["AWS_DEFAULT_REGION=eu-west-1"]) {
                    withCredentials([
                        AWS_CREDENTIALS,
                        string(credentialsId: 'GLOBAL_M11G_INFRASTRUCTURE_ECR_URL', variable: 'M11G_ECR_URL')
                    ]) {
                        def cmd = "aws ecr get-login-password | docker login --username AWS --password-stdin ${env.M11G_ECR_URL}"
                        sh(cmd)
                    }
                }
            }
        }
    }
}
