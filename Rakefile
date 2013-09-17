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
  
  install_textmate_theme if RUBY_PLATFORM.downcase.include?("darwin")
  
  install_textmate_preferences if RUBY_PLATFORM.downcase.include?("darwin")

  install_term_theme if RUBY_PLATFORM.downcase.include?("darwin")

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
    puts "--------------------------------------------------------------------------"
    puts "Installing Homebrew, the OSX package manager...If it's"
    puts "already installed, this will do nothing."
    puts "--------------------------------------------------------------------------"
    run %{ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"}
  end

  puts
  puts
  puts "--------------------------------------------------------------------------"
  puts "Updating Homebrew."
  puts "--------------------------------------------------------------------------"
  run %{brew update}
  puts
  puts
  puts "--------------------------------------------------------------------------"
  puts "Installing Homebrew packages...There may be some warnings."
  puts "--------------------------------------------------------------------------"
  run %{brew install zsh ctags git hub tmux reattach-to-user-namespace coreutils}
  puts
  puts
end

def install_fonts
  puts "--------------------------------------------------------------------------"
  puts "Installing patched fonts for Powerline..."
  puts "--------------------------------------------------------------------------"
  run %{ cp -f $HOME/.qutie/fonts/* $HOME/Library/Fonts }
  puts
end

def install_textmate_theme
  if File.exists?('/Applications/TextMate.app')
    puts "--------------------------------------------------------------------------"
    puts "Installing Base16 themes in your TextMate configuration..."
    puts "--------------------------------------------------------------------------"
    run %{ mkdir -p "$HOME/Library/Application Support/Avian/Bundles/" }
    run %{ cp -R $HOME/.qutie/textmate/Base16.tmbundle "$HOME/Library/Application Support/Avian/Bundles/" }
  end
end

def install_textmate_preferences
  if File.exists?('/Applications/TextMate.app')
    puts "--------------------------------------------------------------------------"
    puts "Customizing preferences of TextMate 2..."
    puts "--------------------------------------------------------------------------"
    run %{ cp -f $HOME/.qutie/textmate/tm_properties "$HOME/.tm_properties" }
  end
end

def install_term_theme
  puts "--------------------------------------------------------------------------"
  puts "Installing Base16 themes in your iTerm 2 configuration..."
  puts "--------------------------------------------------------------------------"
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Bright Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-bright.dark.256.itermcolors' :'Custom Color Presets':'Base16 Bright Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Bright Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-bright.dark.itermcolors' :'Custom Color Presets':'Base16 Bright Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Bright Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-bright.light.256.itermcolors' :'Custom Color Presets':'Base16 Bright Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Bright Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-bright.light.itermcolors' :'Custom Color Presets':'Base16 Bright Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Chalk Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-chalk.dark.256.itermcolors' :'Custom Color Presets':'Base16 Chalk Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Chalk Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-chalk.dark.itermcolors' :'Custom Color Presets':'Base16 Chalk Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Chalk Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-chalk.light.256.itermcolors' :'Custom Color Presets':'Base16 Chalk Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Chalk Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-chalk.light.itermcolors' :'Custom Color Presets':'Base16 Chalk Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }

  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Default Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-default.dark.256.itermcolors' :'Custom Color Presets':'Base16 Default Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Default Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-default.dark.itermcolors' :'Custom Color Presets':'Base16 Default Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Default Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-default.light.256.itermcolors' :'Custom Color Presets':'Base16 Default Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Default Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-default.light.itermcolors' :'Custom Color Presets':'Base16 Default Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Eighties Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-eighties.dark.256.itermcolors' :'Custom Color Presets':'Base16 Eighties Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Eighties Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-eighties.dark.itermcolors' :'Custom Color Presets':'Base16 Eighties Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Eighties Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-eighties.light.256.itermcolors' :'Custom Color Presets':'Base16 Eighties Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Eighties Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-eighties.light.itermcolors' :'Custom Color Presets':'Base16 Eighties Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Greenscreen Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-greenscreen.dark.256.itermcolors' :'Custom Color Presets':'Base16 Greenscreen Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Greenscreen Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-greenscreen.dark.itermcolors' :'Custom Color Presets':'Base16 Greenscreen Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Greenscreen Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-greenscreen.light.256.itermcolors' :'Custom Color Presets':'Base16 Greenscreen Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Greenscreen Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-greenscreen.light.itermcolors' :'Custom Color Presets':'Base16 Greenscreen Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Mocha Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-mocha.dark.256.itermcolors' :'Custom Color Presets':'Base16 Mocha Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Mocha Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-mocha.dark.itermcolors' :'Custom Color Presets':'Base16 Mocha Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Mocha Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-mocha.light.256.itermcolors' :'Custom Color Presets':'Base16 Mocha Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Mocha Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-mocha.light.itermcolors' :'Custom Color Presets':'Base16 Mocha Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Monokai Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-monokai.dark.256.itermcolors' :'Custom Color Presets':'Base16 Monokai Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Monokai Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-monokai.dark.itermcolors' :'Custom Color Presets':'Base16 Monokai Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Monokai Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-monokai.light.256.itermcolors' :'Custom Color Presets':'Base16 Monokai Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Monokai Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-monokai.light.itermcolors' :'Custom Color Presets':'Base16 Monokai Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }

  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Ocean Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-ocean.dark.256.itermcolors' :'Custom Color Presets':'Base16 Ocean Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Ocean Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-ocean.dark.itermcolors' :'Custom Color Presets':'Base16 Ocean Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Ocean Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-ocean.light.256.itermcolors' :'Custom Color Presets':'Base16 Ocean Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Ocean Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-ocean.light.itermcolors' :'Custom Color Presets':'Base16 Ocean Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }

  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Railscasts Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-railscasts.dark.256.itermcolors' :'Custom Color Presets':'Base16 Railscasts Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Railscasts Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-railscasts.dark.itermcolors' :'Custom Color Presets':'Base16 Railscasts Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Railscasts Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-railscasts.light.256.itermcolors' :'Custom Color Presets':'Base16 Railscasts Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Railscasts Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-railscasts.light.itermcolors' :'Custom Color Presets':'Base16 Railscasts Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }

  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Solarized Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-solarized.dark.256.itermcolors' :'Custom Color Presets':'Base16 Solarized Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Solarized Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-solarized.dark.itermcolors' :'Custom Color Presets':'Base16 Solarized Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Solarized Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-solarized.light.256.itermcolors' :'Custom Color Presets':'Base16 Solarized Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Solarized Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-solarized.light.itermcolors' :'Custom Color Presets':'Base16 Solarized Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Tomorrow Dark 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-tomorrow.dark.256.itermcolors' :'Custom Color Presets':'Base16 Tomorrow Dark 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Tomorrow Dark' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-tomorrow.dark.itermcolors' :'Custom Color Presets':'Base16 Tomorrow Dark'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Tomorrow Light 256' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-tomorrow.light.256.itermcolors' :'Custom Color Presets':'Base16 Tomorrow Light 256'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Add :'Custom Color Presets':'Base16 Tomorrow Light' dict" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
  run %{ /usr/libexec/PlistBuddy -c "Merge 'iterm2/base16-tomorrow.light.itermcolors' :'Custom Color Presets':'Base16 Tomorrow Light'" $HOME/Library/Preferences/com.googlecode.iterm2.plist }

  # If iTerm2 is not installed or has never run, we can't autoinstall the profile since the plist is not there
  if !File.exists?(File.join(ENV['HOME'], '/Library/Preferences/com.googlecode.iterm2.plist'))
    puts "--------------------------------------------------------------------------"
    puts "To make sure your profile is using a Base16 theme"
    puts "Please check your settings under:"
    puts "\033[33mPreferences > Profiles > [your profile] > Colors > Load Preset...\033[0m"
    puts "--------------------------------------------------------------------------"
    return
  end

  # Ask the user which theme he wants to install
  message = "Which theme would you like to apply to your iTerm2 profile?"
  color_scheme_file = File.join('iterm2', "base16-eighties.dark.256.itermcolors")

  # Ask the user on which profile he wants to install the theme
  profiles = iTerm_profile_list
  (profiles.size).times { |idx| apply_theme_to_iterm_profile_idx idx, color_scheme_file }
end

def iTerm_available_themes
   Dir['iterm2/*.itermcolors'].map { |value| File.basename(value, '.itermcolors')}
end

def iTerm_profile_list
  profiles=Array.new
  begin
    profiles <<  %x{ /usr/libexec/PlistBuddy -c "Print :'New Bookmarks':#{profiles.size}:Name" $HOME/Library/Preferences/com.googlecode.iterm2.plist 2>/dev/null}
  end while $?.exitstatus==0
  profiles.pop
  profiles
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

def apply_theme_to_iterm_profile_idx(index, color_scheme_path)
  values = Array.new
  16.times { |i| values << "Ansi #{i} Color" }
  values << ['Background Color', 'Bold Color', 'Cursor Color', 'Cursor Text Color', 'Foreground Color', 'Selected Text Color', 'Selection Color']
  values.flatten.each { |entry| run %{ /usr/libexec/PlistBuddy -c "Delete :'New Bookmarks':#{index}:'#{entry}'" $HOME/Library/Preferences/com.googlecode.iterm2.plist } }

  run %{ /usr/libexec/PlistBuddy -c "Merge '#{color_scheme_path}' :'New Bookmarks':#{index}" $HOME/Library/Preferences/com.googlecode.iterm2.plist }
end

def success_msg(action)
  puts "--------------------------------------------------------------------------"
  puts "\033[33mQutie\033[0m has been \033[37m#{action}\033[0m."
  puts "Please restart your terminal and vim."
end