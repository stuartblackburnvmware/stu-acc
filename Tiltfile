LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
WAIT_TIMEOUT = os.getenv("WAIT_TIMEOUT", default='10m00s')
TYPE = os.getenv("TYPE", default='web')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')

k8s_custom_deploy(
    'stu-python-app',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --namespace " + NAMESPACE +
               " --wait-timeout " + WAIT_TIMEOUT +
               " --type " + TYPE +
               " --yes " +
               OUTPUT_TO_NULL_COMMAND +
               " && kubectl get workload stu-python-app --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=[''],
    container_selector='workload',
    live_update=[
      sync('.', '/workspace/'),
      run(
            'pip install -r /workspace/requirements.txt',
            trigger=['./workspace/requirements.txt']
        )
    ]
)

k8s_resource('stu-python-app', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'stu-python-app', 'app.kubernetes.io/component': 'run'}])
