node {
    stage('Build') {
        docker.image('python:2-alpine')withRun('-p 3200:3200')
    }
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
}
