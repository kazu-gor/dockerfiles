FROM python:3
USER root

ENV PATH $PATH:/usr/sbin:/usr/bin:/sbin:/bin:$HOME/.local/bin:/usr/local/bin/pip

# パッケージインストール
RUN apt-get update -y && \
    apt-get install -y \ 
      locales \
      neovim \
      npm \
      yarn \
      fd-find \
      tmux \
      python3-venv && \
    localedef -f UTF-8 -i ja_JP ja_JP.UTF-8 && \
    apt-get -y install --no-install-recommends zsh && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    mv /usr/bin/fdfind /usr/bin/fd && \
    apt-get update -y

# setting dotfiles
RUN git clone https://github.com/kazu-gor/dotfiles.git ~/dotfiles && \
    cp ~/dotfiles/.zshrc_lite ~/.zshrc && \
    cp ~/dotfiles/.zsh_history ~/.zsh_history && \
    cp ~/dotfiles/.tmux.conf ~/.tmux.conf && \
    cp -R ~/dotfiles/.config ~/.config && \
    cp -R ~/dotfiles/.jupyter ~/.jupyter && \
    git clone https://github.com/junegunn/fzf.git ~/.fzf && \
    ~/.fzf/install --key-bindings --completion --update-rc

# vim関連
RUN curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
RUN npm install --global pure-prompt

# pip関連
RUN pip install --upgrade pip
RUN pip install --upgrade setuptools
RUN pip install  --no-cache-dir \
                jupyterlab \
                notebook \
                numpy \
                pandas \
                scipy \
                scikit-learn \
                matplotlib \
                mlxtend \
                seaborn \
                plotly \
                opencv-python 
# Pytorch
RUN pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cpu

# setting jupyter vim-binding
RUN mkdir -p $(jupyter --data-dir)/nbextensions && \
    cd $(jupyter --data-dir)/nbextensions && \
    git clone https://github.com/lambdalisue/jupyter-vim-binding vim_binding && \
    jupyter nbextension enable vim_binding/vim_binding

CMD ["/bin/zsh"]

ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9
ENV TERM xterm

