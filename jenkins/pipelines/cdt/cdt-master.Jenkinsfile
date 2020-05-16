pipeline {
  agent {
    label 'linux'
  }
  options {
    timestamps()
    disableConcurrentBuilds()
  }
  stages {
    stage('Process info') {
      steps {
        container('cdt') {
          timeout(activity: true, time: 20) {
            withEnv(['MAVEN_OPTS=-Xmx768m -Xms768m']) {
                sh "ps -AHf"
                sh "cat /etc/passwd | tail -1"
            }
          }
        }
      }
    }
    stage('Git Clone') {
      steps {
        container('cdt') {
          checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CheckoutOption', timeout: 20], [$class: 'CloneOption', depth: 0, noTags: true, reference: '', shallow: false, timeout: 20]], submoduleCfg: [], userRemoteConfigs: [[url: 'git://git.eclipse.org/gitroot/cdt/org.eclipse.cdt.git']]])
        }
      }
    }
    /* Disabled for experiments */
    /*
    stage('Run build') {
      steps {
        container('cdt') {
          timeout(activity: true, time: 20) {
            withEnv(['MAVEN_OPTS=-Xmx768m -Xms768m']) {
                sh "/usr/share/maven/bin/mvn \
                      clean verify -B -V \
                      -P build-standalone-debugger-rcp \
                      -P baseline-compare-and-replace \
                      -Ddsf.gdb.tests.timeout.multiplier=50 \
                      -Dindexer.timeout=300 \
                      -P production \
                      -Dmaven.repo.local=/home/jenkins/.m2/repository \
                      --settings /home/jenkins/.m2/settings.xml \
                      "
            }
          }
        }
      }
    }
    */
  }
  post {
    always {
      container('cdt') {
    /* Disabled for experiments */
      }
    }
  }
}
