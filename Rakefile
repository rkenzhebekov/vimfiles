task :default => [:plug, :tmp_dirs, :link, :vim]

desc %(Setup plug)
task :plug do
  mkdir_p "plugged"
  sh "curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim"
end

desc %(Create temporary folders)
task :tmp_dirs do
  mkdir_p "_backup"
  mkdir_p "_temp"
end

desc %(Make ~/.vimrc symlink)
task :link do
  dotfile = File.join(ENV['HOME'], ".vimrc")
  if File.exist? dotfile
    warn "#{dotfile} already exists"
  else
    ln_s File.join('.vim', 'vimrc'), dotfile
  end
end

desc %(Run Vim)
task :vim do
  # sh "vim"
  sh "vim +'PlugInstall --sync' +qa"
end

desc %(Check if MacVim is installed)
task :macvim_check do
  if mvim = which('mvim') and '/usr/bin/vim' == which('vim')
    warn color('Warning:', 31) + " You have MacVim installed, but `vim` still opens system Vim."
    warn "To use MacVim version when you invoke `vim`:  " + color("$ ln -s mvim #{File.dirname(mvim)}/vim", 37)
  end
end

def color(msg, code)
  if $stderr.tty? then "\e[1;#{code}m#{msg}\e[m"
  else msg
  end
end

def which(cmd)
  exts = ENV['PATHEXT'] ? ENV['PATHEXT'].split(';') : ['']
  ENV['PATH'].split(File::PATH_SEPARATOR).each do |path|
    exts.each { |ext|
      exe = "#{path}/#{cmd}#{ext}"
      return exe if File.executable? exe
    }
  end
  return nil
end
