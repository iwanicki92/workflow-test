# Runner & Workflow tests

## Notes


1. Build docker image from <https://github.com/actions/runner/tree/v2.321.0>

    ```sh
    docker buildx build . -f images/Dockerfile -t runner:2.321.0 \
        --build-arg TARGETOS=linux --build-arg TARGETARCH=amd64 \
        --build-arg RUNNER_VERSION=2.321.0 --platform=linux/amd64
    ```

1. Run and configure runner

    ```sh
    docker run -it --rm --entrypoint bash runner:2.321.0
    ./config.sh --url https://github.com/iwanicki92/workflow-test --token <token>
    ```

1. Add `--ephemeral` argument if you want runner to automatically unregister and
exit after job finishes

1. Run runner with

    ```sh
    ./run.sh
    ```

1. To run multiple runners run second container instance and then configure &
run second runner. While configuring set unique name e.g.

    ```sh
    ./config.sh --unattended --name runner_1 --ephemeral --url https://github.com/iwanicki92/workflow-test --token <TOKEN>
    ```

1. By default each runner will have `self-hosted` label. If your workflow
contains multiple jobs then each job might run on different runner.

## Ephemeral runners

Might have problems with <https://github.com/actions/runner/issues/1396> if run
from the docker
