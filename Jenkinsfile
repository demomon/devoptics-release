pipeline {
    agent none
    parameters {
        string defaultValue: '', description: 'Frontend Image ID', name: 'FRONTEND_IMAGE_ID', trim: true
    }

    stages {
        stage('Build') {
            agent any
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
                echo "Going to consume: Frontend Image ${FRONTEND_IMAGE_ID}"
                gateConsumesRun jobName: "devoptics/devoptics-deploy/master", runId: "${BACKEND_RUN}"
                gateConsumesArtifact type: "docker", id: "${FRONTEND_IMAGE_ID}"
            }
        }
    }
}
