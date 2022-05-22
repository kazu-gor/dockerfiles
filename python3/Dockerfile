FROM python:3
USER root

ENV PATH $PATH:/usr/sbin:/usr/bin:/sbin:/bin:$HOME/.local/bin

RUN apt-get update -y && \
    apt-get install -y \ 
      locales \
      neovim \
      npm \
      yarn \
      fd-find \
      python3-venv \
      && \
    localedef -f UTF-8 -i ja_JP ja_JP.UTF-8 && \
    apt-get -y install --no-install-recommends zsh && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    mv /usr/bin/fdfind /usr/bin/fd && \
    apt-get update -y


# dotfiles
RUN git clone https://github.com/kazu-gor/dotfiles.git ~/dotfiles && \
    cp ~/dotfiles/.zshrc_lite ~/.zshrc && \
    cp ~/dotfiles/.zsh_history ~/.zsh_history && \
    mkdir ~/.config && \
    cp -R ~/dotfiles/.config .config && \
    git clone https://github.com/junegunn/fzf.git ~/.fzf && \
    ~/.fzf/install --key-bindings --completion --update-rc

# vim関連
RUN curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
RUN npm install --global pure-prompt


CMD ["/bin/zsh"]

ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9
ENV TERM xterm

RUN pip install --upgrade pip
RUN pip install --upgrade setuptools
# RUN python -m pip install jupyterlab \
#                           numpy \
#                           pandas \
#                           matplotlib
# RUN python -m pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cpu
