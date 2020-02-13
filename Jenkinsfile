def label = "my-pipeline-${UUID.randomUUID().toString()}"
 
podTemplate(label: label, containers: [
 containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.8', command: 'cat', ttyEnabled: true),
 containerTemplate(name: 'kaniko', image: 'gcr.io/kaniko-project/executor:debug', command: '/busybox/cat', ttyEnabled: true)
],
volumes: [
   secretVolume(mountPath: '/root/.docker/', secretName: 'dockercred')
]) {
 node(label) {
   stage('Stage 1: Build with Kaniko') {
     container('kaniko') {
       sh '/kaniko/executor --context=git://github.com/ahirshberg/goPlay.git \
               --destination=docker.io/cics89/hirshcorp:latest \
               --insecure \
               --skip-tls-verify  \
               -v=debug'
     }
   }
 
   stage('Stage 2: Run kubectl again') {
     container('kubectl') {
       sh  'kubectl apply -f https://raw.githubusercontent.com/ahirshberg/CNR-examples/master/wepapp-deployment.yaml -n test'
     }
   }
 }
}
