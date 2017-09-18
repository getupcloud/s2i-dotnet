FROM openshift/base-centos7

MAINTAINER Mateus Caruccio <mateus.caruccio@getupcloud.com>

EXPOSE 8080

ENV DOTNET_CORE_VERSION=2.0

LABEL io.k8s.description="Platform for building and running .NET applications" \
      io.k8s.display-name=".NET Core-2.0" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,.net,.netcore,dotnet,dotnetcore,asp.net,asp.netcore"

COPY dotnetdev.repo /etc/yum.repos.d/dotnetdev.repo

RUN INSTALL_PKGS="libunwind libicu" && \
    yum update -y && \
    rpm --import https://packages.microsoft.com/keys/microsoft.asc && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    yum install -y dotnet-sdk-2.0.0 && \
    rpm -V $INSTALL_PKGS dotnet-sdk-2.0.0 && \
    yum clean all -y

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

COPY ./contrib/ /opt/app-root

USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
