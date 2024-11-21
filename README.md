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
