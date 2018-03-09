# Mac Development Ansible Playbook


This playbook installs and configures most of the software I use on my Mac for web and software development.
Some things in macOS are slightly difficult to automate, so I still have some manual installation steps, but at least it's all documented here.

This is a work in progress, and is mostly a means for me to document my current Mac's setup.
I'll be evolving this set of playbooks over time.


## Installation

  1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
  2. [Install pyenv, pipsi, etc ...](https://jacobian.org/writing/python-environment-2018/)
  3. [Install Ansible via pipsi](http://docs.ansible.com/intro_installation.html).
  4. Clone this repository to your local drive.
  5. Run `$ ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
  6. Run `ansible-playbook main.yml -i inventory -K` inside this directory. Enter your account password when prompted.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    ansible-playbook main.yml -i inventory -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file.
For example, you can customize the installed packages and apps with something like:

    homebrew_installed_packages:
      - cowsay
      - git
      - go

    mas_installed_apps:
      - { id: 443987910, name: "1Password" }
      - { id: 498486288, name: "Quick Resizer" }
      - { id: 557168941, name: "Tweetbot" }
      - { id: 497799835, name: "Xcode" }

    composer_packages:
      - name: hirak/prestissimo
      - name: drush/drush
        version: '^8.1'

    gem_packages:
      - name: bundler
        state: latest

    npm_packages:
      - name: webpack

    pip_packages:
      - name: mkdocs

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask):

  - [Dropbox](https://www.dropbox.com/)
  - [Firefox](https://www.mozilla.org/en-US/firefox/new/)
  - [Google Chrome](https://www.google.com/chrome/)
  - [Handbrake](https://handbrake.fr/)
  - [Homebrew](http://brew.sh/)
  - [LICEcap](http://www.cockos.com/licecap/)
  - [LimeChat](http://limechat.net/mac/)
  - [MacVim](http://macvim-dev.github.io/macvim/)
  - [nvALT](http://brettterpstra.com/projects/nvalt/)
  - [Slack](https://slack.com/)
  - [Sublime Text](https://www.sublimetext.com/)
  - [Transmit](https://panic.com/transmit/) (S/FTP client)
  - [Vagrant](https://www.vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  - [Atom](https://atom.io/)
  - [VLC](https://www.videolan.org/vlc/index.html)

Packages (installed with Homebrew):

  - autoconf
  - bash-completion
  - chromedriver
  - doxygen
  - gettext
  - gifsicle
  - git
  - gpg
  - hub
  - iperf
  - libevent
  - mcrypt
  - nmap
  - node
  - nvm
  - ssh-copy-id
  - readline
  - openssl
  - pv
  - wget
  - wrk
  - zlib
  - jpeg
  - libpng
  - libyaml


Finally, there are a few other preferences and settings added on for various apps and services.

## Future additions

### Things that still need to be done manually

It's my hope that I can get the rest of these things wrapped up into Ansible playbooks soon, but for now, these steps need to be completed manually (assuming you already have Xcode and Ansible installed, and have run this playbook).

  1. Install [Sublime Package Manager](http://sublime.wbond.net/installation).
  2. Install all the apps that aren't yet in this setup (see below).
  3. Remap Caps Lock to Escape (requires macOS Sierra 10.12.1+).
  4. Set trackpad tracking rate.
  5. Set mouse tracking rate.
  6. Configure extra Mail and/or Calendar accounts (e.g. Google, Exchange, etc.).
  7. Install [Docker](https://www.docker.com/)
  8. Install [Drone CLI](https://github.com/drone/drone-cli)
  9. Install go with cross-compile action: brew install go --cross-compile-common
 10. Install vim: brew install vim --with-python --with-ruby --with-perl
 11. mac tilling
 12. https://github.com/arialdomartini/oh-my-git

### Applications/packages to be added:

These are mostly direct download links, some are more difficult to install because of custom installers or other nonstandard install quirks:

  - [iShowU HD](http://www.shinywhitebox.com/downloads/iShowU_HD_2.3.20.dmg)
  - [BitBar](https://getbitbar.com/)
  - iTerm2
  - Hyper -> https://justinzimmerman.net/post/switching-from-iterm-to-hyperterm/
  - PowerlineGo -> https://github.com/justjanne/powerline-go

### Configuration to be added:

  - I have vim configuration in the repo, but I still need to add the actual installation:
    ```
    mkdir -p ~/.vim/autoload
    git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
    ```
  - Configure go -> https://mnafian.github.io/2016/10/26/Install-Go-by-Homebrew-OSX.html
  - https://gauravsohoni.wordpress.com/2017/01/01/mac-os-install-golang-with-homebrew/

## ToDo

Tweak macOS settings like:

- https://blog.vandenbrand.org/2016/01/04/how-to-automate-your-mac-os-x-setup-with-ansible/
- https://github.com/geerlingguy/dotfiles/blob/master/.osx
- Use Dracule iterm2 theme -> https://draculatheme.com/

## Author

This is forked from

[Jeff Geerling](http://www.jeffgeerling.com/), 2014 (originally inspired by [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)).
