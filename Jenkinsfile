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
                git branch: 'main', url: 'https://github.com/ajax-pyvovarov-y/test-ecr.git'
                withEnv(["AWS_DEFAULT_REGION=eu-west-1"]) {
                    withCredentials([
                        AWS_CREDENTIALS,
                        string(credentialsId: 'GLOBAL_M11G_INFRASTRUCTURE_ECR_URL', variable: 'ECR_URL')
                    ]) {
                        sh("aws ecr get-login-password | docker login --username AWS --password-stdin ${env.ECR_URL}")
                        sh("docker buildx build --platform linux/amd64 . -t ${env.ECR_URL}m11g-db-api-svc:0.0.1 --push")
                    }
                }
            }
        }
    }
}
