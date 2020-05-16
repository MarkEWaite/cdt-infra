pipeline {
  agent {
    label 'freebsd && !cloud && git.markwaite.net' // Choose a specific FreeBSD computer at my house with rotating discs on ZFS file system
    // kubernetes {
    //   yamlFile 'jenkins/pod-templates/cdt-full-pod-standard.yaml'
    // }
  }
  options {
    timestamps()
    disableConcurrentBuilds()
    // skipDefaultCheckout() // Accept that there is a default checkout - Git Clone step will perform checkout
  }
  stages {
    stage('Process info') {
      steps {
        // container('cdt') {
          timeout(activity: true, time: 20) {
            withEnv(['MAVEN_OPTS=-Xmx768m -Xms768m']) {
                sh "ps -AHf"
                sh "cat /etc/passwd | tail -1"
            }
          }
        // }
      }
    }
    stage('Git Clone') {
      steps {
        // container('cdt') {
          // sh('echo dir contents before deleteDir && ls -al')
          // deleteDir() // Act as though agent is ephemeral
          // sh('echo dir contents after deleteDir && ls -al')
          sh('echo git config before checkout && cat .git/config')
          sh('echo dir contents before checkout && ls -al')
          checkout([$class: 'GitSCM', branches: [[name: '*/master']],  extensions: [[$class: 'CloneOption',  noTags: true, timeout: 20]], userRemoteConfigs: [[url: 'git://git.eclipse.org/gitroot/cdt/org.eclipse.cdt.git']]])
        // }
      }
    }
    // stage('Run build') {
    //   steps {
    //     container('cdt') {
    //       timeout(activity: true, time: 20) {
    //         withEnv(['MAVEN_OPTS=-Xmx768m -Xms768m']) {
    //             sh "/usr/share/maven/bin/mvn \
    //                   clean verify -B -V \
    //                   -P build-standalone-debugger-rcp \
    //                   -P baseline-compare-and-replace \
    //                   -Ddsf.gdb.tests.timeout.multiplier=50 \
    //                   -Dindexer.timeout=300 \
    //                   -P production \
    //                   -Dmaven.repo.local=/home/jenkins/.m2/repository \
    //                   --settings /home/jenkins/.m2/settings.xml \
    //                   "
    //         }
    //       }
    //     }
    //   }
    // }
  }
  // post {
  //   always {
  //     container('cdt') {
  //       junit '*/*/target/surefire-reports/*.xml,terminal/plugins/org.eclipse.tm.terminal.test/target/surefire-reports/*.xml'
  //       archiveArtifacts '**/target/work/data/.metadata/.log,releng/org.eclipse.cdt.repo/target/org.eclipse.cdt.repo.zip,releng/org.eclipse.cdt.repo/target/repository/**,releng/org.eclipse.cdt.testing.repo/target/org.eclipse.cdt.testing.repo.zip,releng/org.eclipse.cdt.testing.repo/target/repository/**,debug/org.eclipse.cdt.debug.application.product/target/product/*.tar.gz,debug/org.eclipse.cdt.debug.application.product/target/products/*.zip,debug/org.eclipse.cdt.debug.application.product/target/products/*.tar.gz,debug/org.eclipse.cdt.debug.application.product/target/repository/**,lsp4e-cpp/org.eclipse.lsp4e.cpp.site/target/repository/**,lsp4e-cpp/org.eclipse.lsp4e.cpp.site/target/org.eclipse.lsp4e.cpp.repo.zip'
  //     }
  //   }
  // }
}
