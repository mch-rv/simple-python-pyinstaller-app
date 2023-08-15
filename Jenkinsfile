node {
    docker.image ('python:2-alpine').inside('-p 3000:3000'){
        stage('Build') {

                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    docker.image ('qnib/pytest').inside('-p 4000:4000'){
        try {
            stage('Test') {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        } finally {
            junit 'test-reports/results.xml'
        }
    }
    docker.image('cdrx/pyinstaller-linux:python2').inside('-p 5000:5000'){
        try {
            stage('Deliver') {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            archiveArtifacts 'dist/add2vals'
        }
    }
}