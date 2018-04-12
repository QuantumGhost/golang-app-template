FROM golang:{{ cookiecutter.go_version }}
ARG COMMIT_ID
ARG VERSION
ARG EXTRA_BUILD_ARGS=""
ARG DOCKER_PROJECT_DIR=/go/src/{{ cookiecutter.package }}
WORKDIR $DOCKER_PROJECT_DIR
COPY . $DOCKER_PROJECT_DIR
RUN mkdir -p /output && \
    make build -e COMMIT_ID=$COMMIT_ID -e VERSION=$VERSION \
    -e OUTPUT_FILE=/output/{{ cookiecutter.package }} -e EXTRA_BUILD_ARGS=$EXTRA_BUILD_ARGS


FROM {{ cookiecutter.base_image }}
WORKDIR /
COPY --from=0 /output/{{ cookiecutter.package }} .
CMD ["./{{ cookiecutter.package }}"]

