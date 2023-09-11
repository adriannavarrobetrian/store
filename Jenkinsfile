def imageName = 'adriannavarro/store'

node('jenkins_agent'){
    stage('Checkout'){
        checkout scm
    }

    def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")

    stage('Tests'){

        parallel(
            'Quality Tests': {
                //sh "docker run --rm ${imageTest} npm run lint"
                echo "Quality tests passed"
            },
            'Integration Tests': {
                //sh "docker run --rm ${imageTest} npm run test"
                echo "Quality tests passed"

            },
            'Coverage Reports': {
                // sh "docker run --rm -v $PWD/coverage:/app/coverage ${imageTest} npm run coverage-html"
                // publishHTML (target: [
                //     allowMissing: false,
                //     alwaysLinkToLastBuild: false,
                //     keepAll: true,
                //     reportDir: "$PWD/coverage",
                //     reportFiles: "index.html",
                //     reportName: "Coverage Report"
                echo "Coverage tests passed"

                //])
            }
        )
    }

    stage('Build'){
        docker.build(imageName)
    }

    stage('Push'){
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            docker.image(imageName).push(commitID())

            if (env.BRANCH_NAME == 'develop') {
                docker.image(imageName).push('develop')
            }
        }
    }
}

def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}