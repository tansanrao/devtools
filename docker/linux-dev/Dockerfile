FROM ubuntu:noble-20240605
ARG UID
ARG GID
ARG USER

ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install --fix-missing -y sudo adduser perl zsh build-essential vim tmux curl clang clangd bear \
	git clang-format libncurses-dev bison flex libssl-dev libelf-dev lld llvm ccache cmake strace bpftrace gdb \
	fakeroot dwarves bc xz-utils debhelper binutils-dev
# QEMU
RUN apt-get install --fix-missing -y qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon attr openssh-client 

# Cleanup
RUN apt-get dist-clean

# Setup current user inside the container
RUN deluser --remove-all-files ubuntu && \
	addgroup --gid $GID $USER && \
	adduser --uid $UID --gid $GID --disabled-password --gecos "" $USER && \
	echo `${USER} ALL=(ALL) NOPASSWD: ALL` >> /etc/sudoers
USER $USER
WORKDIR /home/$USER

# Install omz
RUN sh -c "$(curl https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" "" --unattended
# Install pure
RUN mkdir -p "$HOME/.zsh" && git clone https://github.com/sindresorhus/pure.git "$HOME/.zsh/pure"
# Setup dotfiles
RUN git clone https://github.com/tansanrao/dotfiles.git ~/.config/dotfiles && \
	ln -s ~/.config/dotfiles/tmux/tmux.conf ~/.tmux.conf && \
	ln -s ~/.config/dotfiles/vim/vimrc ~/.vimrc && \
	rm ~/.zshrc && \
	ln -s ~/.config/dotfiles/zshrc ~/.zshrc

# Install tpm
RUN git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm && ~/.tmux/plugins/tpm/bin/install_plugins

# Cleanup

CMD ["tail", "-f", "/dev/null"]
