# Build a small Go program
FROM registry.access.redhat.com/ubi8/ubi:latest as builder
WORKDIR /build
COPY . .
RUN yum -y install go-toolset
RUN go build hello-world.go

FROM quay.io/fedora/fedora-coreos:stable
# Inject it into Fedora CoreOS
COPY --from=builder /build/hello-world /usr/bin
# And add our unit file
ADD hello-world.service /etc/systemd/system/hello-world.service
# Also add strace; the `rm -rf /var/cache` is the equivalent of `yum clean all`.
# For `ostree container commit`, see https://github.com/ostreedev/ostree-rs-ext/issues/159
RUN rpm-ostree install strace && rm -rf /var/cache && \
  ostree container commit
