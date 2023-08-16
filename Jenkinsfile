node {
    withEnv(['VOLUME = $(pwd)/sources:/src',
            'IMAGE = cdrx/pyinstaller-linux:python2']) {
        docker.image('python:2-alpine').inside('-p 3000:3000'){
            stage('Build') {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
    }
    docker.image('qnib/pytest').inside('-p 3100:3100'){
        try {
            stage('Test') {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        } finally {
            junit 'test-reports/results.xml'
        }
    }
     docker.image(${IMAGE}).inside('-p 3100:3100'){
        stage('Deploy') { 
            try {
                dir(path: env.BUILD_ID) { 
                    unstash(name: 'compiled-results') 
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'" 
                }
            } finally {
                archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals" 
                sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
            }
        }
    }
}