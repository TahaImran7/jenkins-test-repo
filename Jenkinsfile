pipeline {
    agent any // Run on any available Jenkins agent/node

    stages {
        stage('1. Create Configuration Files') {
            steps {
                script {
                    // File 1: Simple text file for a version number
                    writeFile(
                        file: 'app_version.txt',
                        text: 'v1.5.0-BETA'
                    )
                    
                    // File 2: A simulated configuration file (e.g., JSON, YAML, or script)
                    writeFile(
                        file: 'deployment_config.json',
                        text: '''
{
  "environment": "staging",
  "port": 8080,
  "service_name": "backend-api"
}
'''
                    )
                    
                    echo "Two files created in the workspace."
                }
            }
        }

        stage('2. Read and Verify Files') {
            steps {
                script {
                    // Use the sh or bat step to list and check the created files
                    echo "Listing files in workspace:"
                    if (isUnix()) {
                        // For Linux/Unix agents
                        sh 'ls -l *.txt *.json'
                        sh 'cat deployment_config.json'
                    } else {
                        // For Windows agents
                        bat 'dir *.txt *.json'
                        bat 'type deployment_config.json'
                    }
                }
            }
        }
        
        stage('3. Use File Contents') {
            steps {
                script {
                    // Use the built-in 'readFile' step to load file content into a variable
                    
                    // Read File 1 content
                    def version = readFile(file: 'app_version.txt').trim()
                    echo "The application version read from file is: ${version}"
                    
                    // Read File 2 content (requires Groovy parsing)
                    def configJson = readFile(file: 'deployment_config.json')
                    
                    // Recommended: Use the 'readJSON' step (if available/installed) 
                    // or a shell tool like 'jq' to parse the content.
                    
                    // For a simple demonstration, we'll just echo the raw content:
                    echo "Using configuration for environment: ${configJson.tokenize('\n').get(2).trim()}"
                    
                    // Example usage in a deployment command
                    sh "echo 'Deploying artifact ${version} to environment ${configJson.tokenize('\n').get(2).trim()}'"
                }
            }
        }
    }
}
