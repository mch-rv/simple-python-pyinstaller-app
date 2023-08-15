node {
    docker.image('python:2-alpine').inside('-p 3000:3000'){
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
}