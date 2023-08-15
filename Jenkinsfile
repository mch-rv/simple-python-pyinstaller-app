node {
    docker.image('python:2-alpine').inside('-p 3000:3000'){
        stage('Build') {

                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
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
    docker.image('cdrx/pyinstaller-linux:python2').inside('-p 3200:3200'){
        try {
            stage('Deploy') {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        } finally {
            archiveArtifacts 'dist/add2vals'
        }
    }
}