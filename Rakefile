require 'rake'
require 'fileutils'
#require File.join(File.dirname(__FILE__), 'bin') #'yadr', 'vundle')

desc "Hook our dotfiles into system-standard positions."
task :install do
  puts "\033[33m   ____        __  _        "
  puts "\033[33m  / __ \\__  __/ /_(_)__     \033[37mqut.ie"
  puts "\033[33m / / / / / / / __/ / _ \\    \033[31m/'kyoote/"
  puts "\033[33m/ /_/ / /_/ / /_/ /  __/    \033[32mNoun"
  puts "\033[33m\\___\\_\\__,_/\\__/_/\\___/     \033[0mAn attractive or endearing shell."
  puts "--------------------------------------------------------------------------"
  puts "Copyright (C) 2013 Emiliano Lesende."
  puts "Based on YADR. Copyright (c) 2011-2012, Yan Pritzker. All rights reserved."
  puts "--------------------------------------------------------------------------\033[0m"

  
  #install_homebrew if RUBY_PLATFORM.downcase.include?("darwin")

  # this has all the runcoms from this directory.
  #file_operation(Dir.glob('git/*')) if want_to_install?('git configs (color, aliases)')
  #file_operation(Dir.glob('irb/*')) if want_to_install?('irb/pry configs (more colorful)')
  #file_operation(Dir.glob('ruby/*')) if want_to_install?('rubygems config (faster/no docs)')
  #file_operation(Dir.glob('ctags/*')) if want_to_install?('ctags config (better js/ruby support)')
  #file_operation(Dir.glob('tmux/*')) if want_to_install?('tmux config')
  #file_operation(Dir.glob('vimify/*')) if want_to_install?('vimification of command line tools')
  
  install_vundle
  
  install_prezto
  
  install_nvm
  
  install_fonts
  
  install_chrome_custom_css
  
  install_textmate_theme
  
  install_textmate_preferences

  install_term_theme

  install_terminal_theme

  success_msg("installed")
end

def install_nvm
  puts "\033[34m===> \033[0mInstalling Node Version Manager (NVM)..."
  
  %x[git clone https://github.com/creationix/nvm.git $HOME/.nvm]
end

def install_vundle
  puts "\033[34m===> \033[0mInstalling Vundle for VIM..."
  
  unless File.exists?(File.join(ENV['HOME'], ".vim", "bundle", "vundle"))
    %x[mkdir -p $HOME/.vim/bundle/vundle]
    %x[git clone https://github.com/gmarik/vundle.git $HOME/.vim/bundle/vundle]
  end

  puts "\033[34m===> \033[0mInstalling a custom .vimrc file..."
  %x[cp -f $HOME/.qutie/vim/vimrc $HOME/.vimrc]

  puts "\033[34m===> \033[0mSetting a list of default vundles..."
  %x[cp -f $HOME/.qutie/vim/vundles.vim $HOME/.vim/vundles.vim]

  puts "\033[34m===> \033[0mInstalling vundles..."
  %x[vim --noplugin -u $HOME/.vim/vundles.vim -N "+set hidden" "+syntax on" +BundleInstall +qall]
  
end

task :default => 'install'

def install_homebrew
  %x[which brew]
  unless $?.success?
    puts "\033[34m===> \033[0mInstalling Homebrew, the OSX package manager... If it's already installed, this will do nothing."
    %x[ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"]
  end

  puts
  puts
  puts "\033[34m===> \033[0mUpdating Homebrew."
  %x[brew update]
  puts
  puts
  puts "\033[34m===> \033[0mInstalling Homebrew packages...There may be some warnings."
  %x[brew install zsh ctags git hub tmux reattach-to-user-namespace coreutils]
  puts
  puts
end

def install_fonts
  puts "\033[34m===> \033[0mInstalling patched fonts for Powerline..."
  %x[cp -f $HOME/.qutie/fonts/* $HOME/Library/Fonts]
end

def install_chrome_custom_css
  puts "\033[34m===> \033[0mInstalling Google Chrome Developer Tools custom CSS..."
  %x[cp -f $HOME/.qutie/chrome/base16-eighties.dark.css "$HOME/Library/Application\ Support/Google/Chrome/Default/User\ StyleSheets/Custom.css"]
end

def install_textmate_theme
  if File.exists?('/Applications/TextMate.app')
    puts "\033[34m===> \033[0mInstalling Base16 themes in your TextMate configuration..."
    %x[mkdir -p "$HOME/Library/Application Support/Avian/Bundles/"]
    %x[cp -R $HOME/.qutie/textmate/Base16.tmbundle "$HOME/Library/Application Support/Avian/Bundles/"]
  end
end

def install_textmate_preferences
  if File.exists?('/Applications/TextMate.app')
    puts "\033[34m===> \033[0mCustomizing preferences of TextMate 2..."
    %x[cp -f $HOME/.qutie/textmate/tm_properties "$HOME/.tm_properties"]
  end
end

def install_terminal_theme
  puts "\033[34m===> \033[0mInstalling Base16 theme in your Terminal.app configuration..."
  %x[/usr/bin/defaults write com.apple.Terminal 'Window Settings' -dict-add 'Base16' '#{File.read("terminal/base16.profile")}']
  %x[/usr/bin/defaults write com.apple.Terminal 'Default Window Settings' 'Base16']
  %x[/usr/bin/defaults write com.apple.Terminal 'Startup Window Settings' 'Base16']
end

def install_term_theme
  puts "\033[34m===> \033[0mInstalling Base16 themes in your iTerm 2 configuration..."
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Bright Dark 256'  '#{File.read("iterm2/base16-bright.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Bright Dark'      '#{File.read("iterm2/base16-bright.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Bright Light 256' '#{File.read("iterm2/base16-bright.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Bright Light'     '#{File.read("iterm2/base16-bright.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Chalk Dark 256'  '#{File.read("iterm2/base16-chalk.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Chalk Dark'      '#{File.read("iterm2/base16-chalk.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Chalk Light 256' '#{File.read("iterm2/base16-chalk.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Chalk Light'     '#{File.read("iterm2/base16-chalk.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Default Dark 256'  '#{File.read("iterm2/base16-default.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Default Dark'      '#{File.read("iterm2/base16-default.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Default Light 256' '#{File.read("iterm2/base16-default.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Default Light'     '#{File.read("iterm2/base16-default.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Eighties Dark 256'  '#{File.read("iterm2/base16-eighties.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Eighties Dark'      '#{File.read("iterm2/base16-eighties.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Eighties Light 256' '#{File.read("iterm2/base16-eighties.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Eighties Light'     '#{File.read("iterm2/base16-eighties.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Greenscreen Dark 256'  '#{File.read("iterm2/base16-greenscreen.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Greenscreen Dark'      '#{File.read("iterm2/base16-greenscreen.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Greenscreen Light 256' '#{File.read("iterm2/base16-greenscreen.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Greenscreen Light'     '#{File.read("iterm2/base16-greenscreen.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Mocha Dark 256'  '#{File.read("iterm2/base16-mocha.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Mocha Dark'      '#{File.read("iterm2/base16-mocha.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Mocha Light 256' '#{File.read("iterm2/base16-mocha.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Mocha Light'     '#{File.read("iterm2/base16-mocha.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Monokai Dark 256'  '#{File.read("iterm2/base16-monokai.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Monokai Dark'      '#{File.read("iterm2/base16-monokai.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Monokai Light 256' '#{File.read("iterm2/base16-monokai.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Monokai Light'     '#{File.read("iterm2/base16-monokai.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Ocean Dark 256'  '#{File.read("iterm2/base16-ocean.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Ocean Dark'      '#{File.read("iterm2/base16-ocean.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Ocean Light 256' '#{File.read("iterm2/base16-ocean.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Ocean Light'     '#{File.read("iterm2/base16-ocean.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Pop Dark 256'  '#{File.read("iterm2/base16-pop.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Pop Dark'      '#{File.read("iterm2/base16-pop.dark.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Railscasts Dark 256'  '#{File.read("iterm2/base16-railscasts.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Railscasts Dark'      '#{File.read("iterm2/base16-railscasts.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Railscasts Light 256' '#{File.read("iterm2/base16-railscasts.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Railscasts Light'     '#{File.read("iterm2/base16-railscasts.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Solarized Dark 256'  '#{File.read("iterm2/base16-solarized.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Solarized Dark'      '#{File.read("iterm2/base16-solarized.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Solarized Light 256' '#{File.read("iterm2/base16-solarized.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Solarized Light'     '#{File.read("iterm2/base16-solarized.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Tomorrow Dark 256'  '#{File.read("iterm2/base16-tomorrow.dark.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Tomorrow Dark'      '#{File.read("iterm2/base16-tomorrow.dark.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Tomorrow Light 256' '#{File.read("iterm2/base16-tomorrow.light.256.itermcolors")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Tomorrow Light'     '#{File.read("iterm2/base16-tomorrow.light.itermcolors")}' ]

  %x[ /usr/bin/defaults write com.googlecode.iterm2 'New Bookmarks' -array-add '#{File.read("iterm2/base16.profile")}' ]
  %x[ /usr/bin/defaults write com.googlecode.iterm2 'Default Bookmark Guid' '0DF677D1-A5C9-40DE-BB66-0342059C18D4' ]

  # If iTerm2 is not installed or has never run, we can't autoinstall the profile since the plist is not there
  if !File.exists?(File.join(ENV['HOME'], '/Library/Preferences/com.googlecode.iterm2.plist'))
    puts "\033[33m--------------------------------------------------------------------------\033[0m"
    puts "\033[33mTo make sure your profile is using a Base16 theme\033[0m"
    puts "\033[33mPlease check your settings under:\033[0m"
    puts "\033[33mPreferences > Profiles > [your profile] > Colors > Load Preset...\033[0m"
    puts "\033[33m--------------------------------------------------------------------------\033[0m"
    return
  end
end

def install_prezto
  puts "\033[34m===> \033[0mInstalling Prezto (ZSH Enhancements)..."
  unless File.exists?(File.join(ENV['ZDOTDIR'] || ENV['HOME'], ".zprezto"))
    %x[/bin/zsh -c 'git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"']
  end
  %x[/bin/zsh -c 'setopt EXTENDED_GLOB; for rcfile in "${ZDOTDIR:-$HOME}"/.zprezto/runcoms/^README.md(.N); do ln -sf "$rcfile" "${ZDOTDIR:-$HOME}/.${rcfile:t}"; done']
  %x[rm -f $HOME/.zpreztorc]
  %x[cp -f $HOME/.qutie/zsh/zpreztorc $HOME/.zpreztorc]
  
  %x[cp -Rf $HOME/.qutie/zsh/modules/base16 $HOME/.zprezto/modules]
  
  if ENV["SHELL"].include? 'zsh' then
    puts "\033[31m===> \033[0mZsh is already configured as your shell of choice. Restart your session to load the new settings"
  else
    puts "\033[34m===> \033[0mSetting zsh as your default shell..."
    %x[chsh -s /bin/zsh]
  end

  puts "\033[34m===> \033[0mInstalling Powerline prompt for ZSH..."
  %x[cp -f $HOME/.qutie/zsh/prompt-powerline $HOME/.zprezto/modules/prompt/functions/prompt_powerline_setup]
end

def want_to_install? (section)
  if ENV["ASK"]=="true"
    puts "Would you like to install configuration files for: #{section}? [y]es, [n]o"
    STDIN.gets.chomp == 'y'
  else
    true
  end
end

def success_msg(action)
  puts "--------------------------------------------------------------------------"
  puts "\033[33mQutie\033[0m has been \033[37m#{action}\033[0m."
  puts "Please restart your terminal and vim."
end
