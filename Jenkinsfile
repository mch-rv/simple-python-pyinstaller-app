node {
     docker.image('python:2-alpine')withRun('-p 3200:3200')
    stage('Build') {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
}