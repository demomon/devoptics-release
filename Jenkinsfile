pipeline {
    agent any
    options {
        durabilityHint 'PERFORMANCE_OPTIMIZED'
    }
    parameters {
        string defaultValue: '', description: 'Frontend Image ID', name: 'FRONTEND_IMAGE_ID'
    }
    stages {
        stage('Front-end Verification') {
            steps {
                echo "Going to consume: Frontend Image ${FRONTEND_IMAGE_ID}"
                gateConsumesArtifact type: "docker", id: "${FRONTEND_IMAGE_ID}"
            }
        }
        stage('Backend Verification') {
            input {
                message 'Which Backend Deployment run?'
                id 'backend-deployment-run'
                ok 'This one'
                parameters {
                    run description: '', filter: 'SUCCESSFUL', name: 'BACKEND_RUN', projectName: 'devoptics/devoptics-deploy/master'
                }
            }
            steps {
                // a run parameter will add _JOBNAME and _NUMBER as additional environment variables
                echo "Going to consume: devoptics-deploy-${BACKEND_RUN}"
                gateConsumesRun jobName: "devoptics/devoptics-deploy/master", runId: "${BACKEND_RUN}"
            }
        }
    }
}
