---
title: Jenkins pipeline
author: kuldeep
datetime: 2022-12-21
slug: jenkins
featured: true
draft: false
tags:
  - jenkins
  - ci/cd
ogImage: ""
description: Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.
---

# Jenkins

## Sample pipeline script
```groovy
pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'make'
            }
        }
        stage('Test'){
            steps {
                sh 'make check'
                junit 'reports/**/*.xml'
            }
        }
        stage('Deploy') {
            steps {
                sh 'make publish'
            }
        }
    }
}
```

## Sample
```groovy
pipeline {
  agent any
  environment {
    // Variable declaration and usage
    def nssmPath = 'C:\\Users\\admin\\Desktop\\kuldeep\\nssm-2.24\\win64\\nssm.exe'
  }
  stages {
    stage('Clean-up') {
      steps {
        bat 'if exist C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Jenkins-job\\dist rmdir /S /Q C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Jenkins-job\\dist'
      }
    }

    stage('Pull new code') {
      steps {
        // Clone repo
        git branch: 'production', credentialsId: 'PAT-Jenkins', url: 'https://github.com/ETN-Solutions/customer-portal-fe.git'
      }
    }

    stage('Install packages') {
      steps {
        // List files
        bat 'npm i'
      }
    }

    stage('Build') {
      steps {
        // List files
        bat 'npm run prod'
      }
    }

    stage('Stop service') {
      steps {
        bat "${nssmPath} stop serve-cp"
      }
    }

    stage('Cleanup') {
      steps {
        // Delete all files inside the public directory
        bat 'if exist C:\\Users\\admin\\Desktop\\kuldeep\\express\\customerPortal\\public\\* del /Q C:\\Users\\admin\\Desktop\\kuldeep\\express\\customerPortal\\public\\*'

        // Delete all subdirectories inside the public directory
        bat 'for /D %%p in ("C:\\Users\\admin\\Desktop\\kuldeep\\express\\customerPortal\\public\\*") do rmdir /S /Q "%%p"'
      }
    }

    stage('Copy Files') {
      steps {
        script {
          // Create the destination directory if it doesn't exist
          bat 'if not exist C:\\Users\\admin\\Desktop\\kuldeep\\express\\customerPortal\\public mkdir C:\\Users\\admin\\Desktop\\kuldeep\\express\\customerPortal\\public'

          // Copy files using xcopy
          bat 'xcopy C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Jenkins-job\\dist\\* C:\\Users\\admin\\Desktop\\kuldeep\\express\\customerPortal\\public /E /H /C /I /Y'
        }
      }
    }

    stage('Start service') {
      steps {
        bat "${nssmPath} start serve-cp"
      }
    }
  }
}
```
