node {
<<<<<<< HEAD
   def appName = 'gceme'
  def feSvcName = "${appName}-frontend"
 ///MODIFICACION
=======
  //def project = 'REPLACE_WITH_YOUR_PROJECT_ID'
  def appName = 'gceme'
  def feSvcName = "${appName}-frontend"
///  def imageTag = "gcr.io/${project}/${appName}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"

>>>>>>> 60adb5379cbed9ead746011ae5e6bab2cb6b5f12
  checkout scm

  stage 'Build image'
  sh ("sudo docker build https://github.com/qemm2/demo.git")
<<<<<<< HEAD
=======
//sh("docker build -t ${imageTag} .")
>>>>>>> 60adb5379cbed9ead746011ae5e6bab2cb6b5f12

  stage 'Run Go tests'
  sh ("sudo docker images -q |head -n 1 > result")
  def output=readFile('result').trim()
  sh("sudo docker run ${output} go test")

<<<<<<< HEAD
=======
//  stage 'Push image to registry'
//  sh("gcloud docker push ${imageTag}")
>>>>>>> 60adb5379cbed9ead746011ae5e6bab2cb6b5f12

  stage "Deploy Application"
  switch (env.BRANCH_NAME) {
    // Roll out to staging
    case "staging":
<<<<<<< HEAD
        sh("kubectl --namespace=produccion apply -f k8s/services/")
        sh("kubectl --namespace=produccion apply -f k8s/staging/")
        sh("echo http://`kubectl --namespace=pro get service/${feSvcName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${feSvcName}")
=======
        // Change deployed image in staging to the one we just built
//        sh("sed -i.bak 's#gcr.io/cloud-solutions-images/gceme:1.0.0#${imageTag}#' ./k8s/staging/*.yaml")
        sh("kubectl --namespace=production apply -f k8s/services/")
        sh("kubectl --namespace=production apply -f k8s/staging/")
        sh("echo http://`kubectl --namespace=production get service/${feSvcName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${feSvcName}")
>>>>>>> 60adb5379cbed9ead746011ae5e6bab2cb6b5f12
        break

    // Roll out to production
    case "master":
<<<<<<< HEAD
        sh("kubectl --namespace=produccion apply -f k8s/services/")
        sh("kubectl --namespace=produccion apply -f k8s/production/")
        sh("echo http://`kubectl --namespace=pro get service/${feSvcName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${feSvcName}")
=======
        // Change deployed image in staging to the one we just built
//        sh("sed -i.bak 's#gcr.io/cloud-solutions-images/gceme:1.0.0#${imageTag}#' ./k8s/production/*.yaml")
        sh("kubectl --namespace=production apply -f k8s/services/")
        sh("kubectl --namespace=production apply -f k8s/production/")
        sh("echo http://`kubectl --namespace=production get service/${feSvcName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${feSvcName}")
>>>>>>> 60adb5379cbed9ead746011ae5e6bab2cb6b5f12
        break

    // Roll out a dev environment
    default:
        // Create namespace if it doesn't exist
<<<<<<< HEAD
=======
//        sh("kubectl get ns ${env.BRANCH_NAME} || kubectl create ns ${env.BRANCH_NAME}")
        // Don't use public load balancing for development branches
>>>>>>> 60adb5379cbed9ead746011ae5e6bab2cb6b5f12
        sh("sed -i.bak 's#LoadBalancer#ClusterIP#' ./k8s/services/frontend.yaml")
        sh("sed -i.bak 's#gcr.io/cloud-solutions-images/gceme:1.0.0#${imageTag}#' ./k8s/dev/*.yaml")
        sh("kubectl --namespace=${env.BRANCH_NAME} apply -f k8s/services/")
        sh("kubectl --namespace=${env.BRANCH_NAME} apply -f k8s/dev/")
        echo 'To access your environment run `kubectl proxy`'
        echo "Then access your service via http://localhost:8001/api/v1/proxy/namespaces/${env.BRANCH_NAME}/services/${feSvcName}:80/"
  }
}
