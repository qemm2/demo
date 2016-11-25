node {
  def appName = 'gceme'
  def feSvcName = "${appName}-frontend"

  checkout scm

  stage 'Build image'
  sh ("sudo docker build https://github.com/qemm2/demo.git")

  stage 'Run Go tests'
  sh ("sudo docker images -q |head -n 1 > result")
  def output=readFile('result').trim()
  sh("sudo docker run ${output} go test")


  stage "Deploy Application"
  switch (env.BRANCH_NAME) {
    // Roll out to staging
    case "staging":
        sh("kubectl --namespace=produccion apply -f k8s/services/")
        sh("kubectl --namespace=produccion apply -f k8s/staging/")
        sh("echo http://`kubectl --namespace=production get service/${feSvcName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${feSvcName}")
        break

    // Roll out to production
    case "master":
        sh("kubectl --namespace=produccion apply -f k8s/services/")
        sh("kubectl --namespace=produccion apply -f k8s/production/")
        sh("echo http://`kubectl --namespace=production get service/${feSvcName} --output=json | jq -r '.status.loadBalancer.ingress[0].ip'` > ${feSvcName}")
        break

    // Roll out a dev environment
    default:
        // Create namespace if it doesn't exist
        sh("sed -i.bak 's#LoadBalancer#ClusterIP#' ./k8s/services/frontend.yaml")
        sh("sed -i.bak 's#gcr.io/cloud-solutions-images/gceme:1.0.0#${imageTag}#' ./k8s/dev/*.yaml")
        sh("kubectl --namespace=${env.BRANCH_NAME} apply -f k8s/services/")
        sh("kubectl --namespace=${env.BRANCH_NAME} apply -f k8s/dev/")
        echo 'To access your environment run `kubectl proxy`'
        echo "Then access your service via http://localhost:8001/api/v1/proxy/namespaces/${env.BRANCH_NAME}/services/${feSvcName}:80/"
  }
}
