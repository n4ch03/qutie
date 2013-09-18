require 'rake'
require 'fileutils'
#require File.join(File.dirname(__FILE__), 'bin') #'yadr', 'vundle')

desc "Hook our dotfiles into system-standard positions."
task :install => [:submodule_init, :submodules] do
  puts
  puts "--------------------------------------------------------------------------"
  puts "\033[33m   ____        __  _        "
  puts "\033[33m  / __ \\__  __/ /_(_)__     \033[37mqut.ie"
  puts "\033[33m / / / / / / / __/ / _ \\    \033[31m/'kyoote/"
  puts "\033[33m/ /_/ / /_/ / /_/ /  __/    \033[32mNoun"
  puts "\033[33m\\___\\_\\__,_/\\__/_/\\___/     \033[0mAn attractive or endearing shell."
  puts "--------------------------------------------------------------------------"
  puts "Copyright (C) 2013 Emiliano Lesende."
  puts "Based on YADR. Copyright (c) 2011-2012, Yan Pritzker. All rights reserved."
  puts "--------------------------------------------------------------------------\033[0m"
  puts

  
  #install_homebrew if RUBY_PLATFORM.downcase.include?("darwin")
  install_rvm_binstubs

  # this has all the runcoms from this directory.
  #file_operation(Dir.glob('git/*')) if want_to_install?('git configs (color, aliases)')
  #file_operation(Dir.glob('irb/*')) if want_to_install?('irb/pry configs (more colorful)')
  #file_operation(Dir.glob('ruby/*')) if want_to_install?('rubygems config (faster/no docs)')
  #file_operation(Dir.glob('ctags/*')) if want_to_install?('ctags config (better js/ruby support)')
  #file_operation(Dir.glob('tmux/*')) if want_to_install?('tmux config')
  #file_operation(Dir.glob('vimify/*')) if want_to_install?('vimification of command line tools')
  #if want_to_install?('vim configuration (highly recommended)')
  #  file_operation(Dir.glob('{vim,vimrc}')) 
  #  Rake::Task["install_vundle"].execute
  #end

  #Rake::Task["install_prezto"].execute

  install_fonts if RUBY_PLATFORM.downcase.include?("darwin")
  
  install_chrome_custom_css if RUBY_PLATFORM.downcase.include?("darwin")
  
  install_textmate_theme if RUBY_PLATFORM.downcase.include?("darwin")
  
  install_textmate_preferences if RUBY_PLATFORM.downcase.include?("darwin")

  install_term_theme if RUBY_PLATFORM.downcase.include?("darwin")

  install_terminal_theme if RUBY_PLATFORM.downcase.include?("darwin")

  success_msg("installed")
end

task :install_prezto do
  if want_to_install?('zsh enhancements & prezto')
    install_prezto
  end
end

task :update do
  Rake::Task["vundle_migration"].execute if needs_migration_to_vundle?
  Rake::Task["install"].execute
  #TODO: for now, we do the same as install. But it would be nice
  #not to clobber zsh files
end

task :submodule_init do
  #unless ENV["SKIP_SUBMODULES"]
  #  run %{ git submodule update --init --recursive }
  #end
end

desc "Init and update submodules."
task :submodules do
  #unless ENV["SKIP_SUBMODULES"]
  #  puts "======================================================"
  #  puts "Downloading YADR submodules...please wait"
  #  puts "======================================================"

  #  run %{
  #    cd $HOME/.yadr
  #    git submodule update --recursive
  #    git clean -df
  #  }
  #  puts
  #end
end

desc "Performs migration from pathogen to vundle"
task :vundle_migration do
  puts "======================================================"
  puts "Migrating from pathogen to vundle vim plugin manager. "
  puts "This will move the old .vim/bundle directory to" 
  puts ".vim/bundle.old and replacing all your vim plugins with"
  puts "the standard set of plugins. You will then be able to "
  puts "manage your vim's plugin configuration by editing the "
  puts "file .vim/vundles.vim"
  puts "======================================================"

  Dir.glob(File.join('vim', 'bundle','**')) do |sub_path|
    run %{git config -f #{File.join('.git', 'config')} --remove-section submodule.#{sub_path}}
    # `git rm --cached #{sub_path}`
    FileUtils.rm_rf(File.join('.git', 'modules', sub_path))
  end
  FileUtils.mv(File.join('vim','bundle'), File.join('vim', 'bundle.old'))
end

desc "Runs Vundle installer in a clean vim environment"
task :install_vundle do
  puts "======================================================"
  puts "Installing vundle."
  puts "The installer will now proceed to run BundleInstall."
  puts "Due to a bug, the installer may report some errors"
  puts "when installing the plugin 'syntastic'. Fortunately"
  puts "Syntastic will install and work properly despite the"
  puts "errors so please just ignore them and let's hope for"
  puts "an update that fixes the problem!"
  puts "======================================================"

  puts ""
  
  run %{
    cd $HOME/.yadr
    git clone https://github.com/gmarik/vundle.git #{File.join('vim','bundle', 'vundle')}
  }

  Vundle::update_vundle
end

task :default => 'install'


private
def run(cmd)
  puts "[Running] #{cmd}"
  `#{cmd}` unless ENV['DEBUG']
end

def install_rvm_binstubs
  puts "--------------------------------------------------------------------------"
  puts "Installing RVM Bundler support. Never have to type"
  puts "bundle exec again! Please use bundle --binstubs and RVM"
  puts "will automatically use those bins after cd'ing into dir."
  puts "--------------------------------------------------------------------------"
  run %{ chmod +x $rvm_path/hooks/after_cd_bundler }
  puts
end

def install_homebrew
  run %{which brew}
  unless $?.success?
    puts "\033[33m===> \033[0mInstalling Homebrew, the OSX package manager... If it's already installed, this will do nothing."
    run %{ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"}
  end

  puts
  puts
  puts "\033[33m===> \033[0mUpdating Homebrew."
  run %{brew update}
  puts
  puts
  puts "\033[33m===> \033[0mInstalling Homebrew packages...There may be some warnings."
  run %{brew install zsh ctags git hub tmux reattach-to-user-namespace coreutils}
  puts
  puts
end

def install_fonts
  puts "\033[33m===> \033[0mInstalling patched fonts for Powerline..."
  run %{ cp -f $HOME/.qutie/fonts/* $HOME/Library/Fonts }
  puts
end

def install_chrome_custom_css
  puts "\033[33m===> \033[0mInstalling Google Chrome custom CSS..."
  run %{ cp -f $HOME/.qutie/chrome/base16-eighties.dark.css "$HOME/Library/Application\ Support/Google/Chrome/Default/User\ StyleSheets/Custom.css" }
end

def install_textmate_theme
  if File.exists?('/Applications/TextMate.app')
    puts "\033[33m===> \033[0mInstalling Base16 themes in your TextMate configuration..."
    run %{ mkdir -p "$HOME/Library/Application Support/Avian/Bundles/" }
    run %{ cp -R $HOME/.qutie/textmate/Base16.tmbundle "$HOME/Library/Application Support/Avian/Bundles/" }
  end
end

def install_textmate_preferences
  if File.exists?('/Applications/TextMate.app')
    puts "\033[33m===> \033[0mCustomizing preferences of TextMate 2..."
    run %{ cp -f $HOME/.qutie/textmate/tm_properties "$HOME/.tm_properties" }
  end
end

def install_terminal_theme
  puts "\033[33m===> \033[0mInstalling Base16 theme in your Terminal.app configuration..."
  run %{ /usr/bin/defaults write com.apple.Terminal 'Window Settings' -dict-add 'Base16' '#{File.read("terminal/base16.profile")}' }
  run %{ /usr/bin/defaults write com.apple.Terminal 'Default Window Settings' 'Base16' }
  run %{ /usr/bin/defaults write com.apple.Terminal 'Startup Window Settings' 'Base16' }
end

def install_term_theme
  puts "\033[33m===> \033[0mInstalling Base16 themes in your iTerm 2 configuration..."
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Bright Dark 256'  '#{File.read("iterm2/base16-bright.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Bright Dark'      '#{File.read("iterm2/base16-bright.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Bright Light 256' '#{File.read("iterm2/base16-bright.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Bright Light'     '#{File.read("iterm2/base16-bright.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Chalk Dark 256'  '#{File.read("iterm2/base16-chalk.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Chalk Dark'      '#{File.read("iterm2/base16-chalk.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Chalk Light 256' '#{File.read("iterm2/base16-chalk.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Chalk Light'     '#{File.read("iterm2/base16-chalk.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Default Dark 256'  '#{File.read("iterm2/base16-default.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Default Dark'      '#{File.read("iterm2/base16-default.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Default Light 256' '#{File.read("iterm2/base16-default.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Default Light'     '#{File.read("iterm2/base16-default.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Eighties Dark 256'  '#{File.read("iterm2/base16-eighties.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Eighties Dark'      '#{File.read("iterm2/base16-eighties.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Eighties Light 256' '#{File.read("iterm2/base16-eighties.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Eighties Light'     '#{File.read("iterm2/base16-eighties.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Greenscreen Dark 256'  '#{File.read("iterm2/base16-greenscreen.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Greenscreen Dark'      '#{File.read("iterm2/base16-greenscreen.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Greenscreen Light 256' '#{File.read("iterm2/base16-greenscreen.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Greenscreen Light'     '#{File.read("iterm2/base16-greenscreen.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Mocha Dark 256'  '#{File.read("iterm2/base16-mocha.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Mocha Dark'      '#{File.read("iterm2/base16-mocha.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Mocha Light 256' '#{File.read("iterm2/base16-mocha.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Mocha Light'     '#{File.read("iterm2/base16-mocha.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Monokai Dark 256'  '#{File.read("iterm2/base16-monokai.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Monokai Dark'      '#{File.read("iterm2/base16-monokai.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Monokai Light 256' '#{File.read("iterm2/base16-monokai.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Monokai Light'     '#{File.read("iterm2/base16-monokai.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Ocean Dark 256'  '#{File.read("iterm2/base16-ocean.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Ocean Dark'      '#{File.read("iterm2/base16-ocean.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Ocean Light 256' '#{File.read("iterm2/base16-ocean.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Ocean Light'     '#{File.read("iterm2/base16-ocean.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Pop Dark 256'  '#{File.read("iterm2/base16-pop.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Pop Dark'      '#{File.read("iterm2/base16-pop.dark.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Railscasts Dark 256'  '#{File.read("iterm2/base16-railscasts.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Railscasts Dark'      '#{File.read("iterm2/base16-railscasts.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Railscasts Light 256' '#{File.read("iterm2/base16-railscasts.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Railscasts Light'     '#{File.read("iterm2/base16-railscasts.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Solarized Dark 256'  '#{File.read("iterm2/base16-solarized.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Solarized Dark'      '#{File.read("iterm2/base16-solarized.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Solarized Light 256' '#{File.read("iterm2/base16-solarized.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Solarized Light'     '#{File.read("iterm2/base16-solarized.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Tomorrow Dark 256'  '#{File.read("iterm2/base16-tomorrow.dark.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Tomorrow Dark'      '#{File.read("iterm2/base16-tomorrow.dark.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Tomorrow Light 256' '#{File.read("iterm2/base16-tomorrow.light.256.itermcolors")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Custom Color Presets' -dict-add 'Base16 Tomorrow Light'     '#{File.read("iterm2/base16-tomorrow.light.itermcolors")}' }

  run %{ /usr/bin/defaults write com.googlecode.iterm2 'New Bookmarks' -array-add '#{File.read("iterm2/base16.profile")}' }
  run %{ /usr/bin/defaults write com.googlecode.iterm2 'Default Bookmark Guid' '0DF677D1-A5C9-40DE-BB66-0342059C18D4' }

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
  puts
  puts "Installing Prezto (ZSH Enhancements)..."

  unless File.exists?(File.join(ENV['ZDOTDIR'] || ENV['HOME'], ".zprezto"))
    run %{ ln -nfs "$HOME/.yadr/zsh/prezto" "${ZDOTDIR:-$HOME}/.zprezto" }

    # The prezto runcoms are only going to be installed if zprezto has never been installed
    file_operation(Dir.glob('zsh/prezto/runcoms/z*'), :copy)
  end

  puts
  puts "Overriding prezto ~/.zpreztorc with YADR's zpreztorc to enable additional modules..."
  run %{ ln -nfs "$HOME/.yadr/zsh/prezto-override/zpreztorc" "${ZDOTDIR:-$HOME}/.zpreztorc" }

  puts
  puts "Creating directories for your customizations"
  run %{ mkdir -p $HOME/.zsh.before }
  run %{ mkdir -p $HOME/.zsh.after }
  run %{ mkdir -p $HOME/.zsh.prompts }

  if ENV["SHELL"].include? 'zsh' then
    puts "Zsh is already configured as your shell of choice. Restart your session to load the new settings"
  else
    puts "Setting zsh as your default shell"
    run %{ chsh -s /bin/zsh }
  end
end

def want_to_install? (section)
  if ENV["ASK"]=="true"
    puts "Would you like to install configuration files for: #{section}? [y]es, [n]o"
    STDIN.gets.chomp == 'y'
  else
    true
  end
end

def file_operation(files, method = :symlink)
  files.each do |f|
    file = f.split('/').last
    source = "#{ENV["PWD"]}/#{f}"
    target = "#{ENV["HOME"]}/.#{file}"

    puts "======================#{file}=============================="
    puts "Source: #{source}"
    puts "Target: #{target}"

    if File.exists?(target) && (!File.symlink?(target) || (File.symlink?(target) && File.readlink(target) != source))
      puts "[Overwriting] #{target}...leaving original at #{target}.backup..."
      run %{ mv "$HOME/.#{file}" "$HOME/.#{file}.backup" }
    end

    if method == :symlink
      run %{ ln -nfs "#{source}" "#{target}" }
    else
      run %{ cp -f "#{source}" "#{target}" }
    end

    # Temporary solution until we find a way to allow customization
    # This modifies zshrc to load all of yadr's zsh extensions.
    # Eventually yadr's zsh extensions should be ported to prezto modules.
    if file == 'zshrc'
      File.open(target, 'a') do |zshrc|
        zshrc.puts('for config_file ($HOME/.yadr/zsh/*.zsh) source $config_file')
      end
    end

    puts "=========================================================="
    puts
  end
end

def needs_migration_to_vundle?
  File.exists? File.join('vim', 'bundle', 'tpope-vim-pathogen')
end


def list_vim_submodules
  result=`git submodule -q foreach 'echo $name"||"\`git remote -v | awk "END{print \\\\\$2}"\`'`.select{ |line| line =~ /^vim.bundle/ }.map{ |line| line.split('||') }
  Hash[*result.flatten]
end

def success_msg(action)
  puts "--------------------------------------------------------------------------"
  puts "\033[33mQutie\033[0m has been \033[37m#{action}\033[0m."
  puts "Please restart your terminal and vim."
end