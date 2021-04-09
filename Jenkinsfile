#!groovy

@Library('jenkinslib') _

def tools = new org.devops.tools()

String workspace = "/opt/jenkins/workspace"

pipeline {
	agent {
		node {
			label "master"
			customWorkspace "${workspace}"
		}
	}

	options {
		timestamps()
		skipDefaultCheckout()
		disableConcurrentBuilds()
		timeout(time: 1,unit: 'HOURS')
	}
	
	parameters {
	    string(name: 'DEPLOY',defaultValue: 'staging',description: '')
	}

	stages {
		stage("GetCode") {  
		    when { environment name: 'test', value: 'abcdef' }
		    
		    input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
		    
			steps {
				timeout(time:10, unit:"MINUTES") {
					script {
					    echo "Hello, ${PERSON}, nice to meet you."
					    
						println("获取代码")
						tools.PrintMsg("获取代码","red")
						
						println("${test}")
						
						mvnHome = tool "m2"
						println(mvnHome)
						sh "${mvnHome}/bin/mvn --version"
						
					}
				}
			}
		}
		
		stage("Parallel Stage") {
			failFast true
			parallel {
				
				stage("Build") {
					steps {
						timeout(time:20, unit:"MINUTES") {
							script {
								println("应用打包")
								tools.PrintMsg("应用打包","blue")
							}
						}
					}
				}
				
				stage("CodeScan") {
					steps {
						timeout(time:30, unit:"MINUTES") {
							script {
								tools.PrintMsg("代码扫描","green")
							}
						}
					}
				}
			}
		}
	}

	post {
		always {
			script {
				println("always")
			}
		}
		
		success {
			script {
				currentBuild.description += "\n 构建成功！"
			}
		}
		
		failure {
			script {
				currentBuild.description += "\n 构建失败！"
			}
		}
		
		aborted {
			script {
				currentBuild.description += "\n 构建取消"
			}
		}
	}
}
