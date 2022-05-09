SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='index.docker.io/wilcockb200/spring-config-server-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='local-path')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'spring-config-server',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
        " --local-path " + LOCAL_PATH +
        " --source-image " + SOURCE_IMAGE +
        " --namespace " + NAMESPACE +
        " --yes >/dev/null" +
        " && kubectl get workload spring-config-server --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f path-to-workload-yaml --namespace " + NAMESPACE + " --yes" ,
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
        sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('spring-config-server', port_forwards=["8080:8080"],
    extra_pod_selectors=[{'serving.knative.dev/service': 'spring-config-server'}])

update_settings( max_parallel_updates = 3 , k8s_upsert_timeout_secs = 300 , suppress_unused_image_warnings = None ) 
