FROM ubuntu

# ============================================================================================================
# ======================================================================== Environment and Installations =====
# ============================================================================================================

# Set environment variables
ENV RUNNING_IN_DOCKER true
ENV LANG enUS.utf-8
ENV TZ "America/New_York"
ENV SHELL /bin/zsh
ENV TERM xterm

RUN apt update && apt upgrade -y

RUN DEBIAN_FRONTEND="noninteractive" \
    apt install -y --no-install-recommends \
    less openssh-server vim curl bat fd-find nmap
RUN DEBIAN_FRONTEND="noninteractive" \
    apt install -y --no-install-recommends \
    wget git tmux tree unzip ncdu htop tzdata inetutils-ping inetutils-traceroute
RUN DEBIAN_FRONTEND="noninteractive" \
    apt install -y --no-install-recommends \
    python3-setuptools python3-pip python3 ipython3 iproute2 python3-venv

# ============================================================================================================
# ============================================================================================================
# ============================================================================================================


# ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


# ============================================================================================================
# ============================================================================ Shell Customization Stuff =====
# ============================================================================================================

# Install zsh and oh my zsh
RUN apt install -y --no-install-recommends zsh
RUN curl -L http://install.ohmyz.sh | sh
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k 2>/dev/null
RUN sed -i "s/robbyrussell/powerlevel10k\/powerlevel10k/" ~/.zshrc
RUN git clone https://github.com/zsh-users/zsh-autosuggestions.git ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions 2>/dev/null
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting 2>/dev/null
RUN sed -i "s/plugins=/plugins=(git zsh-autosuggestions zsh-syntax-highlighting) #/" ~/.zshrc
# Install cli color scheme
RUN git clone https://github.com/seebi/dircolors-solarized.git dirco_for_script_color_option 2>/dev/null
RUN cp dirco_for_script_color_option/dircolors.256dark ~/.oh-my-zsh/dircolors.256dark
RUN rm -rf dirco_for_script_color_option/
# Install tmux plugins
RUN git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm 2>/dev/null
RUN git clone https://github.com/jimeh/tmux-themepack.git ~/.tmux-themepack
# Append conf files tmux conf and zsh rc
RUN wget https://raw.githubusercontent.com/Tanq16/cli-productivity-suite/master/tmuxconf 2>/dev/null
RUN mv tmuxconf ~/.tmux.conf
RUN wget https://raw.githubusercontent.com/Tanq16/cli-productivity-suite/master/add_to_rc 2>/dev/null
RUN cat add_to_rc >> ~/.zshrc
RUN rm add_to_rc
RUN cp ~/.zshrc temptemp
RUN cat temptemp | grep -vE "^#" | grep -vE "^$" > ~/.zshrc
RUN rm temptemp
RUN echo "export PATH=$PATH:/usr/local/go/bin/" >> ~/.zshrc
RUN chsh -s /usr/bin/zsh
# Setup fzf
RUN git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf 2>/dev/null
RUN ~/.fzf/install --all 1>/dev/null 2>/dev/null
RUN echo "[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh" >> ~/.zshrc
# Install colored ls
RUN wget https://github.com/Peltoche/lsd/releases/download/0.20.1/lsd_0.20.1_amd64.deb
RUN apt install -y ./lsd_0.20.1_amd64.deb && rm lsd_0.20.1_amd64.deb

# Install vim extensions
RUN git clone https://github.com/itchyny/lightline.vim ~/.vim/pack/plugins/start/lightline
RUN git clone https://github.com/jiangmiao/auto-pairs.git ~/auto_pairs_vim
RUN mkdir -p ~/.vim/plugin
RUN cp ~/auto_pairs_vim/plugin/auto-pairs.vim ~/.vim/plugin/
RUN rm -rf ~/auto_pairs_vim
RUN mkdir -p ~/.vim/pack/plugins/start
RUN git clone --depth=1 https://github.com/ervandew/supertab.git ~/.vim/pack/plugins/start/supertab
RUN wget https://raw.githubusercontent.com/dylnmc/novum.vim/master/colors/novum.vim
RUN mkdir -p ~/.vim/colors
RUN mv novum.vim ~/.vim/colors/novum.vim
RUN wget https://raw.githubusercontent.com/Tanq16/cli-productivity-suite/master/.vimrcfile
RUN mv .vimrcfile ~/.vimrc
RUN sleep 2

# ============================================================================================================
# ============================================================================================================
# ============================================================================================================


# ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


# ============================================================================================================
# ========================================================================== External Tools Installation =====
# ============================================================================================================

# Use commands here --
RUN apt download ripgrep && dpkg --force-overwrite -i ripgrep*.deb

# ============================================================================================================
# ============================================================================================================
# ============================================================================================================


# ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


# Final steps for perfect run
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
RUN echo 'root:docker' | chpasswd 
COPY ./p10k.zsh .
RUN mv /p10k.zsh ~/.p10k.zsh
