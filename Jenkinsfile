node('maven') {
  def nameSpace=demo-${env.BRANCH_NAME}
  withEnv(["KUBERNETES_NAMESPACE=${nameSpace}]) {

  System.getProperties().put("proxySet", "true");
  System.getProperties().put("http.proxyHost", "wpad.local.bbsas.no");
  System.getProperties().put("http.proxyPort", "8080");
  System.getProperties().put("https.proxyHost", "wpad.local.bbsas.no");
  System.getProperties().put("https.proxyPort", "8080");
  System.getProperties().put("http.nonProxyHosts", "gerrit.bbsas.no|172.30.36.155|172.30.104.185|localhost|127.0.0.1|*.bbsas.no|source-ng|*.local.bbsas.no|*.default.svc|*.default.svc.cluster.local|*.fabricdemo.svc.cluster.local|docker-registry.default.svc.cluster.local|10.1.*|172.30.*|172.32.*");

  stage 'stage'
  // Create namespace if it doesn't exist
  echo 'Checking if namespace ${nameSpace} derived from branch exists, if not then create it'
  sh("oc get ns ${nameSpace} || oc create ns ${nameSpace}")
  echo 'Running Maven build with Fabric8 integration and staging in local environment'
  sh 'mvn -B -DproxySet=true -DproxyHost=wpad.local.bbsas.no -DproxyPort=8080 -Dfabric8.mode=openshift clean install spring-boot:build-info fabric8:build fabric8:deploy'
  }
}
