FROM registry.fedoraproject.org/fedora:latest

# Don't include container-selinux and remove
# directories used by yum that are just taking
# up space.
RUN dnf -y update; yum -y reinstall shadow-utils; \
yum -y install podman fuse-overlayfs --exclude container-selinux; \
rm -rf /var/cache /var/log/dnf* /var/log/yum.*

RUN useradd podman; \
echo podman:10000:5000 > /etc/subuid; \
echo podman:10000:5000 > /etc/subgid;

VOLUME /var/lib/containers
VOLUME /home/podman/.local/share/containers

ADD https://raw.githubusercontent.com/containers/image_build/main/podman/containers.conf /etc/containers/containers.conf
ADD https://raw.githubusercontent.com/containers/image_build/main/podman/podman-containers.conf /home/podman/.config/containers/containers.conf
ADD https://raw.githubusercontent.com/containers/podman/main/vendor/github.com/containers/storage/storage.conf /etc/containers/storage.conf

RUN chown podman:podman -R /home/podman

# chmod containers.conf and adjust storage.conf to enable Fuse storage.
RUN chmod 644 /etc/containers/containers.conf; sed -i -e 's|^#mount_program|mount_program|g' -e '/additionalimage.*/a "/var/lib/shared",' -e 's|^mountopt[[:space:]]*=.*$|mountopt = "nodev,fsync=0"|g' /etc/containers/storage.conf
RUN mkdir -p /var/lib/shared/overlay-images /var/lib/shared/overlay-layers /var/lib/shared/vfs-images /var/lib/shared/vfs-layers; touch /var/lib/shared/overlay-images/images.lock; touch /var/lib/shared/overlay-layers/layers.lock; touch /var/lib/shared/vfs-images/images.lock; touch /var/lib/shared/vfs-layers/layers.lock


# JAVA Environment
RUN dnf -y install maven

# Mandrel(GraalVM) Environment
RUN dnf -y install glibc-devel zlib-devel gcc freetype-devel libstdc++-static
RUN curl -OL https://github.com/graalvm/mandrel/releases/download/mandrel-23.1.2.0-Final/mandrel-java21-linux-aarch64-23.1.2.0-Final.tar.gz; tar xvf mandrel-java21-linux-aarch64-23.1.2.0-Final.tar.gz; 
ENV JAVA_HOME="${PWD}/mandrel-java21-23.1.2.0-Final"
ENV GRAALVM_HOME="${JAVA_HOME}"
ENV PATH="${JAVA_HOME}/bin:${PATH}"

ENV _CONTAINERS_USERNS_CONFIGURED=""

# Quarkus Environment
RUN curl -Ls https://sh.jbang.dev | bash -s - trust add https://repo1.maven.org/maven2/io/quarkus/quarkus-cli/
RUN curl -Ls https://sh.jbang.dev | bash -s - app install --fresh --force quarkus@quarkusio