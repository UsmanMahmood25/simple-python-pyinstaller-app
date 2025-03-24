// pipeline {
//     agent {
//         docker { image 'python:3.8' }
//     }
//     stages {
//         stage('Build') { 
//             steps {
//                 sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
//                 stash(name: 'compiled-results', includes: 'sources/*.py*') 
//             }
//         }
//         stage('Test') {
//             agent {
//                 docker {
//                     image 'qnib/pytest'
//                 }
//             }
//             steps {
//                 sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
//             }
//             post {
//                 always {
//                     junit 'test-reports/results.xml'
//                 }
//             }
//         }
//     }
// }



pipeline {
    agent none  // No global agent
    stages {
        stage('Build') { 
            agent { docker { image 'python:3.8' } }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') {
            agent { docker { image 'python:3.8' } }
            steps {
                unstash 'compiled-results'  // Restore compiled files
                sh 'pip install pytest'  // Ensure pytest is available
                sh 'pytest --verbose --junit-xml=test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }
}
