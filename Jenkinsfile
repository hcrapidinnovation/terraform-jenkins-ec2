  pipeline {
    agent {
      node {
        label "master"
      } 
    }

     environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID_RAJEE')
        AWS_SECRET_ACCESS_KEY = credentials('SECRET_RAJEE_KEY')
    }


    stages {
      stage('fetch_latest_code') {
        steps {
          git url: 'https://github.com/hcrapidinnovation/terraform-jenkins-ec2'
        }
      }

      stage('TF Init&Plan') {
        steps {
          
          sh 'terraform init -input=false'
          sh 'terraform workspace select ${environment} || terraform workspace new ${environment}'
           sh "terraform plan -input=false -out tfplan "
           sh 'terraform show -no-color tfplan > tfplan.txt'        }      
      }

      stage('Approval') {
        steps {
          script {
            def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
          }
        }
      }

      stage('TF Apply') {
        steps {
          sh 'terraform apply -input=false'
        }
      }
    } 
  }
