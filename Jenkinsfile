#!/usr/bin/env groovy

@Library('piper-library-os') _

node() {
	stage('Prepare') {
		deleteDir()
		checkout scm
		setupCommonPipelineEnvironment script: this
	}

	stage('Build') {
		mtaBuild script: this, buildTarget: 'CF', dockerImage: 'ppiper/mta-archive-builder:latest'
	}

	stage('Deploy') {
		cloudFoundryDeploy script: this, deployTool: 'mtaDeployPlugin'
	}

	stage('Test') {
		newmanExecute script: this, newmanEnvironment: 'integration/ProductCatalog.postman_environment.json', failOnError: false
		testsPublishResults script: this, junit: [pattern: '**/newman/TEST-*.xml']
	}
}
