# Update the ELIXIR_VERSION arg in docker-compose.yml to pick an Elixir version
ARG ELIXIR_VERSION=latest
FROM elixir:${ELIXIR_VERSION}

# Adds a non-root user with sudo access. Update the “remoteUser” property in
# devcontainer.json to use it. More info: https://aka.ms/vscode-remote/containers/non-root-user
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # Add sudo support
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Create a working directory
RUN mkdir -p /src/app
WORKDIR /src/app

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y

# Set the default user
USER $USERNAME

# Executed default command
CMD ["bash"]