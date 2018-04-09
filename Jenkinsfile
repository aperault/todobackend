node {
    checkout scm
    
    try{
        stage 'Run unit/IT tests'
        sh 'make test'
        
        stage 'Build application artifacts'
        sh 'make build'
        
        stage 'Create release environment and run acceptance tests'
        sh 'make release'
        
        stage 'Tag and publish release image'
        sh "make tag latest \$(git rev-parse --short HEAD) \$(git tag --points-at HEAD)"
        sh "make buildtag master \$(git tag --points-at HEAD)"
        print "DEBUG: parameter user = ${DOCKER_USER}"
        print "DEBUG: parameter foo = ${DOCKER_PASSWORD}"
        
        sh "make USER=${DOCKER_USER} PASSWORD=${DOCKER_PASSWORD} login"
        
        
        sh 'make publish'
        
    }
    finally{
        stage 'Collect test reports'
        step([$class: 'JUnitResultArchiver', testResults: '**/reports/*.xml'])
    
        stage 'Clean up'
        sh 'make clean'
        sh 'make logout '
        
    }
}
