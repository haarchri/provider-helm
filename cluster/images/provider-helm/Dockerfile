FROM busybox AS unzipper

ARG TARGETOS
ARG TARGETARCH

ADD https://github.com/Azure/kubelogin/releases/download/v0.0.33/kubelogin-${TARGETOS}-${TARGETARCH}.zip /tmp/kubelogin.zip

RUN unzip /tmp/kubelogin.zip -d /tmp

FROM gcr.io/distroless/static@sha256:1f580b0a1922c3e54ae15b0758b5747b260bd99d39d40c2edb3e7f6e2452298b

ARG TARGETOS
ARG TARGETARCH

# distroless does not allow root level folder creations, which helm tries to do.
ENV HELM_CACHE_HOME /tmp/helm-cache
ENV HELM_CONFIG_HOME /tmp/helm-config
ENV HELM_DATA_HOME /tmp/helm-data

ADD bin/$TARGETOS\_$TARGETARCH/provider /usr/local/bin/provider-helm
# Copy the extracted files from the unzipper stage
COPY --from=unzipper /tmp/bin/$TARGETOS\_$TARGETARCH/kubelogin /usr/local/bin/kubelogin

USER 65532
ENTRYPOINT ["provider-helm"]
