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

1. To make sure jobs run on the same runner set unique label for each runner
e.g. by adding `--labels runner_1` in `config.sh` script.

    ```yaml
    first-job:
        name: First job
        runs-on: self-hosted
        outputs:
            runner: ${{ runner.name }}
    second-job:
        name: Second job
        needs: first-job
        runs-on: ${{ needs.first-job.outputs.runner }}
    ```

    * If runner is ephemeral and recreated in clean container each time then it
    won't work unless using persistent storage. There is also chance that
    another job or workflow might run on this runner between
    `first-job`/`second-job`

1. To share artifacts e.g. build output between jobs you might need to use
`actions/upload-artifact` and `actions/download-artifact` or if runners are on
the same machine then maybe put your output in unique path in shared folder and
pass this path between jobs.

## Ephemeral runners

Might have problems with <https://github.com/actions/runner/issues/1396> if run
from the docker
test
