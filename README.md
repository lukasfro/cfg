# cfg

My dotfiles and configurations.

See [this article](https://www.atlassian.com/git/tutorials/dotfiles) that explains the setup of the repository.

## TL;DR

### Starting from scratch

```bash
git init --bare $HOME/.cfg
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
config config --local status.showUntrackedFiles no
echo "alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'" >> $HOME/.zshrc
```

### Setup a new system

```bash
# https
git clone --bare https://github.com/lukasfro/cfg.git $HOME/.cfg
# ssh
git clone --bare git@github.com:lukasfro/cfg.git $HOME/.cfg
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
config checkout
if [ $? = 0 ]; then
  echo "Checked out config.";
  else
    mkdir -p .config-backup
    echo "Backing up pre-existing dot files.";
    config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | xargs -I{} mv {} .config-backup/{}
fi;
config checkout
config config status.showUntrackedFiles no
source $HOME/.zshrc
```
