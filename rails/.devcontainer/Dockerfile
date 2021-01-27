ARG RUBY_VERSION=3.0 
FROM ruby:${RUBY_VERSION}

LABEL maintainer="Yannick Grillet <grillety@gmail.com>"

ARG APP_HOME=/usr/src/app
ARG NODE_VERSION=lts
ARG RAILS_VERSION=6.1.1
ARG USER_UID=1000
ARG USER_GID=1000
ARG USER_NAME=user

# Add non-root user with sudo support
RUN groupadd --gid ${USER_GID} ${USER_NAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USER_NAME} \
    && apt-get update -yqq \
    && apt-get install -yqq sudo \
    && echo ${USER_NAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USER_NAME} \
    && chmod 0440 /etc/sudoers.d/${USER_NAME}

# Install librairies and tools
RUN apt-get update -yqq \
    && apt-get install -yqq \
        autoconf \
        bison \
        build-essential \
        curl \
        g++ \
        gcc \
        git \
        libffi-dev \
        libgconf-2-4 \
        libgdbm-dev \
        libncurses5-dev \
        libreadline-dev \
        libsqlite3-dev \
        libssl-dev \
        libxi6 \
        libyaml-dev \
        make \
        sqlite3 \
        xvfb \
        zip \
        zlib1g-dev

# Install nodejs
RUN curl -sL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - \
    && apt-get update -yqq \
    && apt-get install -yqq --no-install-recommends \
        nodejs 

# Install yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update -yqq \
    && apt-get install -yqq --no-install-recommends \
        yarn

# Set the working directory
RUN mkdir -p ${APP_HOME} \
    && chown -R ${USER_NAME}:${USER_NAME} ${APP_HOME}
WORKDIR ${APP_HOME}

# Set non-root user as default user
USER ${USER_NAME}

# Install rails
RUN gem install rails -v ${RAILS_VERSION} --no-document

EXPOSE 3000
CMD [ "bash" ]