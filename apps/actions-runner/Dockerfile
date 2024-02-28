ARG VERSION
FROM --platform=amd64 ghcr.io/actions/actions-runner:${VERSION}

ENV HOMEBREW_NO_ANALYTICS=1 \
  HOMEBREW_NO_ENV_HINTS=1 \
  HOMEBREW_NO_INSTALL_CLEANUP=1 \
  DEBCONF_NONINTERACTIVE_SEEN=true \
  DEBIAN_FRONTEND="noninteractive" \
  APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn

USER root

RUN \
  apt-get update && apt-get upgrade -y \
  && \
  apt-get install -y --no-install-recommends \
  acl apt-transport-https aria2 autoconf automake binutils bison brotli bzip2 \
  ca-certificates coreutils curl dbus dnsutils dpkg dpkg-dev fakeroot file \
  findutils flex fonts-noto-color-emoji ftp g++ gcc git gnupg2 gpg-agent \
  haveged imagemagick iproute2 iputils-ping jo jq lib32z1 libc++-dev \
  libc++abi-dev libc6-dev libcurl4 libgbm-dev libgconf-2-4 libgsl-dev \
  libgtk-3-0 libmagic-dev libmagickcore-dev libmagickwand-dev libsecret-1-dev \
  libsqlite3-dev libssl-dev libtool libunwind8 libxkbfile-dev libxss1 \
  libyaml-dev locales lz4 m4 make mediainfo mercurial moreutils net-tools \
  netcat openssh-client p7zip-full p7zip-rar parallel pass patchelf pigz \
  pkg-config pollinate python-is-python3 rpm rsync shellcheck \
  software-properties-common sphinxsearch sqlite3 ssh sshpass subversion sudo \
  swig tar telnet texinfo time tk tzdata unrar unzip upx wget xorriso xvfb \
  xz-utils zip zsync \
  && rm -rf /var/lib/apt/lists/*

# Install additional repositories
RUN wget https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb && \
  dpkg -i packages-microsoft-prod.deb && \
  wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
  echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list && \
  apt-get update -y

# Common Ruby dependencies
RUN apt-get update && apt-get install --no-install-recommends -y \
  google-chrome-stable \
  postgresql-client \
  redis \
  libvips \
  libpq-dev \
  && rm -rf /var/lib/apt/lists/*

ENV RUNNER_TOOL_CACHE=/opt/hostedtoolcache
RUN mkdir ${RUNNER_TOOL_CACHE} \
  && chgrp docker ${RUNNER_TOOL_CACHE} \
  && chmod g+rwx ${RUNNER_TOOL_CACHE}

USER runner

RUN /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
