FROM release:artifacts as artifacts
FROM release:machine-config-operator as mcd

FROM fedora:37
ARG CRIO_VERSION=1.25

WORKDIR /rpms

RUN \
    dnf install -y dnf-plugins-core && \
    dnf module enable -y cri-o:${CRIO_VERSION:-1.25} && \
    mkdir -p /rpms && \
    dnf download -y cri-o cri-tools --destdir /rpms && \
    dnf clean all

COPY --from=artifacts /srv/repo/$([ -d /tmp/rpms/$(uname -m) ] && echo $(uname -m)/)*.rpm /rpms/

# TODO: check: is it really needed
COPY --from=mcd /usr/bin/machine-config-daemon /binaries/machine-config-daemon

COPY manifests /
