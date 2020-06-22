load('ext://restart_process', 'docker_build_with_restart')

# Need to change image based on prompt
k8s_yaml(kustomize('install/admiral/overlays/demosinglecluster'))

docker_build_with_restart(
    'docker.io/admiralproj/admiral',
    '.',
    entrypoint=["dlv", "exec", "--headless", "--listen=:2345", "--api-version=2", "--accept-multiclient", "/usr/local/bin/admiral", "--"],
    dockerfile='admiral/docker/Dockerfile.admiral-dev',
    live_update=[
        sync('admiral/', '/go/src/github.com/istio-ecosystem/admiral'),
        run(['make', 'build-linux']),
    ]
)

k8s_resource('admiral', port_forwards=['2345:2345'])

include('Tiltfile.local')
