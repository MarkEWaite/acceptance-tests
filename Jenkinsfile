#!/usr/bin/env groovy

/*
 * Making sure that we can follow the steps necessary to install the latest
 * release RPM for redhat/centos machine:
 */

properties([
    [$class: 'jenkins.model.BuildDiscarderProperty',
        strategy: [$class: 'LogRotator', numToKeepStr: '5']],
    pipelineTriggers([[$class: 'SCMTrigger', scmpoll_spec: '@hourly']]),
])

node('docker') {
    timestamps {
        docker.image('centos').inside('--rm') {
            stage('Prepare Container') {
                sh 'yum install -y wget'
            }

            stage('Add the rpm key') {
                sh 'wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo'
                sh 'rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key'
            }

            stage('Install Jenkins from rpm') {
                sh 'yum install -y jenkins'
            }
        }
    }
}
