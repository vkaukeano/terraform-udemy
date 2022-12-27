# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.217.4/containers/debian/.devcontainer/base.Dockerfile

# [Choice] Debian version (use bullseye on local arm64/Apple Silicon): bullseye, buster
ARG VARIANT="buster"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

ARG GCLOUD_VERSION="400.0.0"
ARG GCLOUD_ARCH="x86_64"
 # install gcloud
RUN cd /usr/share \
    && curl https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${GCLOUD_VERSION}-linux-${GCLOUD_ARCH}.tar.gz | tar xzf - \
    && echo NY | sh google-cloud-sdk/install.sh \
    && ln -s ${PWD}/google-cloud-sdk/bin/* /usr/bin \
    && echo Y | sudo gcloud components install gke-gcloud-auth-plugin \
    # # install cloud sql proxy
    # && curl -L https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -o /usr/bin/cloud_sql_proxy \
    # && chmod +x /usr/bin/cloud_sql_proxy \
    # # install tfsec
    # && curl -Lo /usr/bin/tfsec-checkgen https://github.com/aquasecurity/tfsec/releases/download/v${TFSEC_VERSION}/tfsec-checkgen-linux-${TFSEC_ARCH} \
    # && curl -Lo /usr/bin/tfsec https://github.com/aquasecurity/tfsec/releases/download/v${TFSEC_VERSION}/tfsec-linux-${TFSEC_ARCH} \
    # && chmod +x /usr/bin/tfsec* \
    # # install kubectl
    && curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl \
    && sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl \
    # # # install Helm
    && curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 \
    && chmod +x get_helm.sh \
    && ./get_helm.sh \
    && rm get_helm.sh \
    #Install bash completions
    && mkdir /home/vscode/.bash_completion.d \
    && cp \
        /home/vscode/.oh-my-zsh/plugins/gitfast/git-completion.bash \
        /usr/share/google-cloud-sdk/completion.bash.inc \
        /home/vscode/.bash_completion.d \
    && echo 'for file in /home/vscode/.bash_completion.d/* ; do source "$file"; done'  >> /home/vscode/.bashrc \
    && echo "alias gauth='gcloud auth login --update-adc'" >> /home/vscode/.bashrc \
    && echo 'complete -C /usr/bin/terraform terraform' >>  /home/vscode/.bashrc
RUN git config --global core.editor "code -w"
# # ** [Optional] Uncomment this section to install additional packages. **
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends php7.4 php7.4-mysql mariadb-client

# # With php and wp cli installed, configure the cli for the local db
# RUN mkdir -p /usr/share/wp /home/vscode/.wp-cli/ /home/vscode/.config/gcloud/ \
#     && chown vscode:vscode /usr/share/wp \
#     && sudo -u vscode -i -- \
#        wp core download \
#           --path=/usr/share/wp \
#     && cd /usr/share/wp \
#     && sed -e "/\(SALT\|KEY\|DB_NAME\|DB_USER\|DB_PASSWORD\|DB_HOST\)/d" wp-config-sample.php > wp-config.php \
#     && curl -L https://api.wordpress.org/secret-key/1.1/salt/ >> wp-config.php \
#     && echo "define( 'DB_USER', 'wordpress' );" >> wp-config.php \
#     && echo "define( 'DB_NAME', 'wordpress' );" >> wp-config.php \
#     && echo "define( 'DB_PASSWORD', 'password' );" >> wp-config.php \
#     && echo "define( 'DB_HOST', '127.0.0.1' );" >> wp-config.php \
#     && echo "path: /usr/share/wp" > /home/vscode/.wp-cli/config.yml \
#     && chown -R vscode:vscode /home/vscode/.config/

# Needed for gcloud to pull docker images from GCP
RUN echo 'export LD_LIBRARY_PATH=/usr/local/lib' >>  /home/vscode/.bashrc
RUN echo 'export PS1="\w -> "' >>  /home/vscode/.bashrc
