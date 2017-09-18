FROM openshift/base-centos7

MAINTAINER Mateus Caruccio <mateus.caruccio@getupcloud.com>

EXPOSE 8080

ENV DOTNET_CORE_VERSION=1.1 \
    DOTNET_CENTOS_TAR_GZ=https://download.microsoft.com/download/E/7/8/E782433E-7737-4E6C-BFBF-290A0A81C3D7/dotnet-dev-centos-x64.1.0.4.tar.gz

LABEL io.k8s.description="Platform for building and running .NET applications" \
      io.k8s.display-name=".NET Core-1.1" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,.net,.netcore,dotnet,dotnetcore,asp.net,asp.netcore"

# Install Apache httpd and PHP
RUN INSTALL_PKGS="libunwind libicu" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    mkdir -p /opt/dotnet && \
    curl -sSL "$DOTNET_CENTOS_TAR_GZ" | tar zx -C /opt/dotnet && \
    ln -s /opt/dotnet/dotnet /usr/local/bin && \
    yum clean all -y

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

COPY ./contrib/ /opt/app-root

USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
