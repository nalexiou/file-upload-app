// Declarative pipelines must be enclosed with a "pipeline" directive.
pipeline {
    // This line is required for declarative pipelines. Just keep it here.
    agent fargate-jenkins-agent

    // This section contains environment variables which are available for use in the
    // pipeline's stages.
    environment {
	    region = "us-east-1"
        docker_repo_uri = "982468706400.dkr.ecr.us-east-1.amazonaws.com/file-app"
		    task_def_arn = "arn:aws:ecs:us-east-1:982468706400:task-definition/file-app"
        cluster = "TestingCluster"
        exec_role_arn = "arn:aws:iam::982468706400:role/ecsTaskExecutionRole"
        def dockerHome = tool 'docker'
        PATH = "${dockerHome}/bin:/var/jenkins_home/bin:${PATH}"

    }

    // Here you can define one or more stages for your pipeline.
    // Each stage can execute one or more steps.
    stages {
        // This is a stage.
        stage('Build') {
            steps {
                // Get SHA1 of current commit
                script {
                    //commit_id = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                    commit_id = "latest"
                }
                // Build the Docker image
                sh 'docker login --username AWS --password $(/var/jenkins_home/bin/aws ecr get-login-password --region us-east-1) 982468706400.dkr.ecr.us-east-1.amazonaws.com'
                sh "docker build -t ${docker_repo_uri}:${commit_id} ."
                // Get Docker login credentials for ECR
                sh "aws ecr get-login --no-include-email --region ${region} | sh"
                // Push Docker image
                sh "docker push ${docker_repo_uri}:${commit_id}"
                // Clean up
                sh "docker rmi -f ${docker_repo_uri}:${commit_id}"
            }
        }

        stage('Deploy') {
        steps {
        // Override image field in taskdef file
        //sh "sed -i 's|{{image}}|${docker_repo_uri}:${commit_id}|' taskdef.json"
        // Create a new task definition revision
        //sh "aws ecs register-task-definition --execution-role-arn ${exec_role_arn} --cli-input-json file://taskdef.json --region ${region}"
        // Update service on Fargate with new task definition
        //sh "aws ecs update-service --cluster ${cluster} --service file-app-service --task-definition ${task_def_arn} --region ${region}"
        // Force redeploy on Existing service
        sh "aws ecs update-service --cluster  ${cluster} --service file-app-service --force-new-deployment"
    }
}
    }
}
