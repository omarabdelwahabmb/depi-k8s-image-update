pipeline {
    agent any

    parameters {
        string(defaultValue: '/home/omar/Eng/Courses/DEPI/Technical/Lec30/k8s-ansible', description: 'Please, Enter project path', name: 'ppath')
        string(defaultValue: 'k8s-nginx', description: 'Please, Enter image name:', name: 'image')
    }
    
    stages {
        stage('Checkout') {
           steps {
                dir ("${params.ppath}") { 
                    git branch: 'main', url: 'https://github.com/omarabdelwahabmb/depi-k8s-image-update.git'
                }
           }
        }
        stage('Apply') {
          steps {
            dir ("${params.ppath}") {
              sh "docker build -t ${params.image} ."
              sh "sed -i -E 's,ppath:.*,ppath: \"${params.ppath}\",' '${params.ppath}/vars/vars.yaml'"
              sh "sed -i -E 's,image:.*,image: \"${params.image}\",' '${params.ppath}/vars/vars.yaml'"
              sh "sed -i -E 's,image:.*,image: \"${params.image}\",' '${params.ppath}/nginx-depl.yaml'"
              sh "ansible-playbook nginx-playbook.yml"
            }
          }
        }
    }
}