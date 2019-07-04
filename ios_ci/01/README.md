[TOC]



## 1. iOS 构建工具离开不了 ruby

| 构建工具  | 开发语言 |
| --------  | ----- |
| bundler   | ruby |
| homebrew  | ruby |
| cocoapods | ruby |
| fastlane  | ruby |
| shenzhen  | ruby |
| fir-cli   | ruby |

很多很多的工具都是 ruby 编写的, 所以在 iOS 构建领域几乎是离不开 ruby 这门语言了.



## 2. 在你的机器上, 可能存在 ==各种各样(不同版本、不同路径)== 的 ruby (可执行文件)

### 1. macos 系统默认安装的 ruby

```
/usr/bin/ruby
```

### 2. brew install 安装的 ruby

```
/usr/local/Cellar/ruby/2.6.2/bin
```

### 3. rvm install 安装的 ruby

```
/Users/xiongzenghui/.rvm/rubies/ruby-2.4.1/bin/ruby
/Users/xiongzenghui/.rvm/rubies/ruby-2.5.1/bin/ruby
/Users/xiongzenghui/.rvm/rubies/ruby-2.6.0/bin/ruby
```

### 4. 甚至自己手动 clone 源码构建生成并安装的 ruby

```
git clone https://github.com/ruby/ruby.git
./configure
make
make install DESTDIR=/path/to prefix=xzh
```

最终 ruby 的目录是这样:

```shell
${DESTDIR}${prefix}/bin
```



## 3. 不同的 ruby , 对应存储 ==lib/gems 目录== 也是不一样的

### 1. macos 系统默认安装的 ruby

#### 1. 打印 ruby 路径

```
 ~  /usr/bin/ruby -v
ruby 2.3.7p456 (2018-03-28 revision 63024) [universal.x86_64-darwin18]
```

#### 2. 进入 lib 目录

```
cd /usr/lib/ruby
```

#### 3. lib 目录内容

```
 /usr/lib/ruby  ll
total 0
drwxr-xr-x  106 root  wheel   3.3K  8 18  2018 2.3.0
drwxr-xr-x    3 root  wheel    96B  8 18  2018 gems
lrwxr-xr-x    1 root  wheel    44B 10  6  2018 site_ruby -> ../../../../../../../../../Library/Ruby/Site
drwxr-xr-x    3 root  wheel    96B  8 18  2018 vendor_ruby
```

#### 4. 最终安装 gem 的 gems 目录

```
 /usr/lib/ruby/gems/2.3.0/gems  ll
total 0
drwxr-xr-x   4 root  wheel   128B  8 18  2018 CFPropertyList-2.2.8
drwxr-xr-x  11 root  wheel   352B  8 18  2018 libxml-ruby-2.9.0
drwxr-xr-x  19 root  wheel   608B  8 18  2018 nokogiri-1.5.6
drwxr-xr-x  15 root  wheel   480B  8 18  2018 sqlite3-1.3.11
```

### 2. brew install 安装的 ruby

#### 1. 打印 ruby 路径

```
 ~  /usr/local/Cellar/ruby/2.6.2/bin/ruby -v
ruby 2.6.2p47 (2019-03-13 revision 67232) [x86_64-darwin18]
```

#### 2. 进入 lib 目录

```
cd /usr/local/Cellar/ruby/2.6.2/lib
```

#### 3. lib 目录内容

```
 /usr/local/Cellar/ruby/2.6.2/lib  ll
total 5144
-rw-r--r--  1 xiongzenghui  staff   2.5M  3 15 00:13 libruby.2.6.dylib
lrwxr-xr-x  1 xiongzenghui  staff    17B  3 13 04:26 libruby.dylib -> libruby.2.6.dylib
drwxr-xr-x  3 xiongzenghui  staff    96B  3 15 00:13 pkgconfig
drwxr-xr-x  4 xiongzenghui  staff   128B  3 13 04:26 ruby
```

#### 4. 最终安装 gem 的 gems 目录

```
 /usr/local/Cellar/ruby/2.6.2/lib/ruby/gems/2.6.0/gems  ll
total 0
drwxr-xr-x   3 xiongzenghui  staff    96B  3 13 04:26 bundler-1.17.2
drwxr-xr-x   3 xiongzenghui  staff    96B  3 13 04:26 bundler-1.17.3
drwxr-xr-x  16 xiongzenghui  staff   512B  3 13 04:26 did_you_mean-1.3.0
drwxr-xr-x   3 xiongzenghui  staff    96B  3 13 04:26 irb-1.0.0
drwxr-xr-x  10 xiongzenghui  staff   320B  3 13 04:26 minitest-5.11.3
drwxr-xr-x  11 xiongzenghui  staff   352B  3 13 04:26 net-telnet-0.2.0
drwxr-xr-x  13 xiongzenghui  staff   416B  3 13 04:26 power_assert-1.1.3
drwxr-xr-x  14 xiongzenghui  staff   448B  3 13 04:26 rake-12.3.2
drwxr-xr-x   3 xiongzenghui  staff    96B  3 13 04:26 rdoc-6.1.0
drwxr-xr-x  12 xiongzenghui  staff   384B  3 13 04:26 test-unit-3.2.9
drwxr-xr-x  11 xiongzenghui  staff   352B  3 13 04:26 xmlrpc-0.3.0
```

### 3. rvm install 安装的 ruby

#### 1. 打印 ruby 路径

```
/Users/xiongzenghui/.rvm/rubies/ruby-2.6.0/bin/ruby
```

#### 2. 进入 lib 目录

```
cd /Users/xiongzenghui/.rvm/rubies/ruby-2.6.0/lib
```

#### 3. lib 目录内容

```
 ~/.rvm/rubies/ruby-2.6.0/lib  ll
total 7504
-rwxr-xr-x  1 xiongzenghui  staff   3.7M  3 15 00:23 libruby.2.6.dylib
lrwxr-xr-x  1 xiongzenghui  staff    17B  3 15 00:24 libruby.dylib -> libruby.2.6.dylib
drwxr-xr-x  3 xiongzenghui  staff    96B  3 15 00:24 pkgconfig
drwxr-xr-x  6 xiongzenghui  staff   192B  3 15 00:24 ruby
```

#### 4. 最终安装 gem 的 gems 目录

```
 ~/.rvm/rubies/ruby-2.6.0/lib/ruby/gems/2.6.0/gems  ll
total 0
drwxr-xr-x   3 xiongzenghui  staff    96B  3 15 00:24 bundler-1.17.2
drwxr-xr-x  16 xiongzenghui  staff   512B  3 15 00:24 did_you_mean-1.3.0
drwxr-xr-x   3 xiongzenghui  staff    96B  3 15 00:24 irb-1.0.0
drwxr-xr-x  10 xiongzenghui  staff   320B  3 15 00:24 minitest-5.11.3
drwxr-xr-x  11 xiongzenghui  staff   352B  3 15 00:24 net-telnet-0.2.0
drwxr-xr-x  13 xiongzenghui  staff   416B  3 15 00:24 power_assert-1.1.3
drwxr-xr-x  14 xiongzenghui  staff   448B  3 15 00:24 rake-12.3.2
drwxr-xr-x   3 xiongzenghui  staff    96B  3 15 00:24 rdoc-6.1.0
drwxr-xr-x  12 xiongzenghui  staff   384B  3 15 00:24 test-unit-3.2.9
drwxr-xr-x  11 xiongzenghui  staff   352B  3 15 00:24 xmlrpc-0.3.0
```

### 4. gem env 查看 ==当前使用的== ruby 环境参数

```
 ~  gem env
RubyGems Environment:
  - RUBYGEMS VERSION: 3.0.1
  - RUBY VERSION: 2.6.0 (2018-12-25 patchlevel 0) [x86_64-darwin18]
  - INSTALLATION DIRECTORY: /Users/xiongzenghui/.rvm/gems/ruby-2.6.0
  - USER INSTALLATION DIRECTORY: /Users/xiongzenghui/.gem/ruby/2.6.0
  - RUBY EXECUTABLE: /Users/xiongzenghui/.rvm/rubies/ruby-2.6.0/bin/ruby
  - GIT EXECUTABLE: /usr/bin/git
  - EXECUTABLE DIRECTORY: /Users/xiongzenghui/.rvm/gems/ruby-2.6.0/bin
  - SPEC CACHE DIRECTORY: /Users/xiongzenghui/.gem/specs
  - SYSTEM CONFIGURATION DIRECTORY: /Users/xiongzenghui/.rvm/rubies/ruby-2.6.0/etc
  - RUBYGEMS PLATFORMS:
    - ruby
    - x86_64-darwin-18
  - GEM PATHS:
     - /Users/xiongzenghui/.rvm/gems/ruby-2.6.0
     - /Users/xiongzenghui/.rvm/rubies/ruby-2.6.0/lib/ruby/gems/2.6.0
  - GEM CONFIGURATION:
     - :update_sources => true
     - :verbose => true
     - :backtrace => false
     - :bulk_threshold => 1000
     - :sources => ["https://rubygems.org/"]
  - REMOTE SOURCES:
     - https://rubygems.org/
  - SHELL PATH:
     - /Users/xiongzenghui/.rvm/gems/ruby-2.6.0/bin
     - /Users/xiongzenghui/.rvm/gems/ruby-2.6.0@global/bin
     - /Users/xiongzenghui/.rvm/rubies/ruby-2.6.0/bin
     - /Users/xiongzenghui/.rvm/bin
     - /Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code
     - /Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code
     - /Users/xiongzenghui/.pyenv/shims
     - /Users/xiongzenghui/.pyenv/bin
     - /Users/xiongzenghui/.oh-my-zsh/custom/plugins/git-open
     - /usr/local/bin
     - /usr/bin
     - /bin
     - /usr/sbin
     - /sbin
     - /usr/local/go/bin
     - /Applications/Wireshark.app/Contents/MacOS
     - /usr/local/mysql/bin
     - /Users/xiongzenghui/mongodb-osx-x86_64-4.0.4
     - /Users/xiongzenghui/mongodb-osx-x86_64-4.0.4/bin
     - /Users/xiongzenghui/llvm
     - /Users/xiongzenghui/llvm_build
```

### 5. 总结

- 使用不同的 **ruby** 就对应不用的 **lib、gems**
- 如果出现各种 **找不到** xx gem 的时候，就要查看是否使用 **错** ruby
  - 1) ruby **路径**
  - 2) ruby **版本**



## 4. 推荐使用 rvm 管理 ruby、gemset

可参考 ruby-china 写的 rmv 使用指南:

```
https://ruby-china.org/wiki/rvm-guide
```

就不重复写了.



## 5. 如果 ==rvm 安装的 ruby== 在 ==jenkins 触发的 shell== 中找到的还是 ==系统 ruby==

### 1. 在机器上 rvm 安装、并使用 ==2.4.1 ruby==

```
rvm use 2.4.1 --default
```

### 2. 但是在 jenkins 触发的 shell 中, 找到的还是 ==系统 ruby==

这是在 jenkins console 中打印的 ruby 版本:

```
+ ruby -v
ruby 2.3.7p456 (2018-03-28 revision 63024) [universal.x86_64-darwin18]
```

并不是在 ci 机器上安装和使用的 **2.4.1** ruby

### 3. 尝试在 jenkins 触发的 shell 中加上 `rvm use 2.4.1 --default` , 但是报错 ==无法找到 rvm==

```
+ rvm use 2.4.1 --default
RVM is not a function, selecting rubies with 'rvm use ...' will not work.
You need to change your terminal emulator preferences to allow login shell.
Sometimes it is required to use `/bin/bash --login` as the command.
Please visit https://rvm.io/integration/gnome-terminal/ for an example.
```

### 4. 查阅到 jenkins 与 rvm 找到的解决

https://codeday.me/bug/20180809/215349.html

```
source $(rvm env 1.9.3@rails-3.2.3 --path)
```

### 5. 那么对于我 rvm 管理的 ==2.4.1 ruby== 则这样 source

```
source $(rvm env ruby-2.4.1@global --path)
```

### 6. 而如果 rvm 创建并使用其他的 gemset, 则这样 source

```
source $(rvm env ruby-<1.版本号>@<2.gemset名字> --path)
```

需要自己按情况填充两个量:

- 1) ruby 版本号
- 2) gemset 名字

### 7. 重新触发 jenkins job 正确打印 ==2.4.1 ruby==

```
++ rvm env ruby-2.4.1@global --path
+ source /Users/zhihu/.rvm/environments/ruby-2.4.1
+ export 'PATH=/Users/zhihu/.rvm/gems/ruby-2.4.1/bin:/Users/zhihu/.rvm/gems/ruby-2.4.1@global/bin:/Users/zhihu/.rvm/rubies/ruby-2.4.1/bin:/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home/bin:/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/zhihu/.rvm/bin'
+ ruby -v
ruby 2.4.1p111 (2017-03-22 revision 58053) [x86_64-darwin18]
```

可以看到添加到 PATH 中的 2.4.1 ruby 相关的 环境变量

```
/Users/zhihu/.rvm/gems/ruby-2.4.1/bin: \
/Users/zhihu/.rvm/gems/ruby-2.4.1@global/bin: \
/Users/zhihu/.rvm/rubies/ruby-2.4.1/bin
```

注意: 一定要在 jenkins 服务的 mac 机器上执行 `rvm use 2.4.1 --default` 指定全局默认使用的 ruby.



## 6. 我经常记混的几种安装 ruby gem 的形式

### 1. gem install <gem 组件名> => ==单个== 安装 gem 组件

```
gem install cocoapods
```

### 2. app项目/Gemfile => ==批量== 安装 gem 组件

#### 1. app项目/Gemfile

```ruby
source 'https://rubygems.org/'

gem 'cocoapods', '1.5'
gem 'xcodeproj', '1.5.7'
gem 'psych', '2.1'
gem 'gitlab', '~> 4.0'
gem 'gli', '~> 2.0'
gem 'plist', '3.4.0'

#.............. 其他需要的 gem 依赖..............
```

#### 2. app项目/ 下执行 bundle install 批量安装 gem

```
 ~/Desktop  vim Gemfile
 ~/Desktop  bundle install
Fetching gem metadata from https://rubygems.org/........
Resolving dependencies...
Using bundler 1.16.4
Using psych 2.1.0
Bundle complete! 1 Gemfile dependency, 2 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
 ~/Desktop 
```

#### 3. 会在 bundle install 目录下生成 Gemfile.lock 文件 (锁死 gem 版本号)

```ruby
GEM
  remote: https://rubygems.org/
  specs:
    psych (2.1.0)

PLATFORMS
  ruby

DEPENDENCIES
  psych (= 2.1)

BUNDLED WITH
   1.16.4
```

- 1) 记录从哪个 gem 【仓库】
- 2) 记录每一个 gem 【安装的版本】
- 3) bundler 版本


### 3. gem/xx.podspec 中的 dependency => gem 依赖其他的 gem

> https://github.com/CocoaPods/CocoaPods/blob/master/cocoapods.gemspec

```ruby
# encoding: UTF-8
require File.expand_path('../lib/cocoapods/gem_version', __FILE__)
require 'date'

Gem::Specification.new do |s|
  s.name     = "cocoapods"
  s.version  = Pod::VERSION
  s.date     = Date.today
  s.license  = "MIT"
  s.email    = ["eloy.de.enige@gmail.com", "fabiopelosin@gmail.com", "kyle@fuller.li", "segiddins@segiddins.me"]
  s.homepage = "https://github.com/CocoaPods/CocoaPods"
  s.authors  = ["Eloy Duran", "Fabio Pelosin", "Kyle Fuller", "Samuel Giddins"]

  s.summary     = "The Cocoa library package manager."
  s.description = "CocoaPods manages library dependencies for your Xcode project.\n\n"     \
                  "You specify the dependencies for your project in one easy text file. "  \
                  "CocoaPods resolves dependencies between libraries, fetches source "     \
                  "code for the dependencies, and creates and maintains an Xcode "         \
                  "workspace to build your project.\n\n"                                   \
                  "Ultimately, the goal is to improve discoverability of, and engagement " \
                  "in, third party open-source libraries, by creating a more centralized " \
                  "ecosystem."

  s.files = Dir["lib/**/*.rb"] + %w{ bin/pod bin/sandbox-pod README.md LICENSE CHANGELOG.md }

  s.executables   = %w{ pod sandbox-pod }
  s.require_paths = %w{ lib }

  # Link with the version of CocoaPods-Core
  s.add_runtime_dependency 'cocoapods-core',        "= #{Pod::VERSION}"

  s.add_runtime_dependency 'claide',                '>= 1.0.2', '< 2.0'
  s.add_runtime_dependency 'cocoapods-deintegrate', '>= 1.0.3', '< 2.0'
  s.add_runtime_dependency 'cocoapods-downloader',  '>= 1.2.2', '< 2.0'
  s.add_runtime_dependency 'cocoapods-plugins',     '>= 1.0.0', '< 2.0'
  s.add_runtime_dependency 'cocoapods-search',      '>= 1.0.0', '< 2.0'
  s.add_runtime_dependency 'cocoapods-stats',       '>= 1.0.0', '< 2.0'
  s.add_runtime_dependency 'cocoapods-trunk',       '>= 1.3.1', '< 2.0'
  s.add_runtime_dependency 'cocoapods-try',         '>= 1.1.0', '< 2.0'
  s.add_runtime_dependency 'molinillo',             '~> 0.6.6'
  s.add_runtime_dependency 'xcodeproj',             '>= 1.10.0', '< 2.0'

  ## Version 5 needs Ruby 2.2, so we specify an upper bound to stay compatible with system ruby
  s.add_runtime_dependency 'activesupport', '>= 4.0.2', '< 5'
  s.add_runtime_dependency 'colored2',       '~> 3.1'
  s.add_runtime_dependency 'escape',        '~> 0.0.4'
  s.add_runtime_dependency 'fourflusher',   '>= 2.3.0', '< 3.0'
  s.add_runtime_dependency 'gh_inspector',  '~> 1.0'
  s.add_runtime_dependency 'nap',           '~> 1.0'
  s.add_runtime_dependency 'ruby-macho',    '~> 1.4'

  s.add_development_dependency 'bacon', '~> 1.1'
  s.add_development_dependency 'bundler', '~> 1.3'
  s.add_development_dependency 'rake', '~> 10.0'

  ## Make sure you can build the gem on older versions of RubyGems too:
  s.rubygems_version = "1.6.2"
  s.required_rubygems_version = Gem::Requirement.new(">= 0") if s.respond_to? :required_rubygems_version=
  s.required_ruby_version = '>= 2.0.0'
  s.specification_version = 3 if s.respond_to? :specification_version
end
```

- 通过 `Gem::Specification#add_runtime_dependency` 方法

- 添加了很多的 **gem 依赖**

- 也就是会说 **当安装 cocoapods** 这个 gem 时, 会自动同时下载安装 **cocoapods 依赖的其他所有 gem**
  - claide
  - cocoapods-deintegrate
  - xcodeproj
  - molinillo
  - activesupport
  - ..... 等等



## 7. 切换 gem 下载源

### 方式1. gem sources => 全局

#### 1. 删除 rubygem 官方源

```
gem sources --remove https://rubygems.org/
```

#### 2. 再添加 rubychina 三方源

```
gem sources -a https://gems.ruby-china.org/
```

#### 3. 但是需要修改 ==app项目/Gemfile== 使用 rubychina 三方源

```ruby
# 删除 rubygem 官方源
#source "https://rubygems.org"

# 使用替换后的 rubychina 三方源
source "https://gems.ruby-china.org/"

git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

# Specify your gem's dependencies in kanshan.gemspec
gemspec
```

#### 4. bundle install 从 rubychina 服务器 下载安装 gem 源码包

- 此时再执行 **bundle install** 
- 就不再从 **rubygem 官方源** 下载 xx.gemspec
- 而是从 【rubychina 服务器】下载【xx.gemspec】

### 方式2. bundle config => 局部

#### 1. 这种方式比较方便

- 1) 不用删除【rubygem 官方】提供的 **gemspec 文件** 下载源
- 2) 只需要在 **执行 bundle install 之前** 临时修改为 **第三方 gemspec 文件下载源** 即可

#### 2. 只需要在【执行 bundle 之前】，临时修改为【第三方 gemspec 文件下载源】

```
bundle config [rubygem官方源地址] [替换成的 gemspec 源地址]
```

#### 3. 具体使用

```
bundle config mirror.https://rubygems.org https://gems.ruby-china.org/
```

**不用** 再去修改 **Gemfile** 文件指定为第三方的下载源。



## 8. bundler

### 1. bundler 有点类似 cocoapods/pod 依赖解析器 (包管理器)

#### 1. rubygems

- 1) 编写 Gemfile 
- 2) bundle install
- 3) 生成 Gemfile.lock

#### 2. cocoapods

- 1) 编写 Podfile 
- 2) pod install
- 3) 生成 Podfile.lock

所以说 cocoapods 其实是模拟实现了 rubygems 功能.

### 2. 安装 bundler

```
gem install bundler
```

### 3. Gemfile 添加 gem 依赖的语法

https://bundler.io/v2.0/guides/using_bundler_in_applications.html

### 4. ==bundle exec==

- 1) 当在机器上安装了 **不同版本** 的 **gem** 时，会出现很多问题
- 2) 在运行 rake（或rspec或其他）之后，可能会收到警告，例如：

```
You have already activated rake 10.3.1, but your Gemfile requires rake 10.1.0. Prepending "bundle exec" to your command may solve this.
```

- 1) 当前使用 **10.3.1** 版本的 gem
- 2) 但是在 Gemfile 中指定的是 **10.1.0** 版本的 gem

所以必须指定运行 **当前 bundle 环境内的版本** 的 gem 软件包 ，而不会出现版本问题。

```
bundle exec ks g m
bundle exec ks g c kanshan.yml
```

- 1) 首先找到 **当前** 使用的 **ruby 解释器** 对应的 **bundler**
- 2) **bundler** 实际上又 **一一对应** 唯一的 **gems 安装目录**
- 3) 通过 **bundler(bundle) exec**  运行的都是 **当前相关 gems 安装目录** 中的 gem 

### 5. group 隔离 gem 依赖

https://bundler.io/v2.0/guides/groups.html

### 6. 一定要提交 Gemfile.lock 的修改

https://bundler.io/v2.0/guides/bundler_sharing.html

在其他的机器上，必须安装 Gemfile.lock 中 **锁死** 的版本的 gem.

### 7. 更新 gem 

https://bundler.io/v2.0/guides/updating_gems.html

- 类似于 **pod update** 安装新的 pod, 都是会修改 Podfile.lock 新的版本
- **bundle update** 也是一样

### 8. 在一个 ruby 脚本中, 使用 bundler

https://bundler.io/v2.0/guides/bundler_in_a_single_file_ruby_script.html

### 9. bundler plugin

https://bundler.io/v2.0/guides/bundler_plugins.html

#### 1. Gemfile 中指定安装 plugin

```ruby
plugin 'my_plugin' # Installs from Rubygems
plugin 'my_plugin', path: '/path/to/plugin' # Installs from a path
plugin 'my_plugin', git: 'https://github.com:repo/my_plugin.git' # Installs from Git
```

#### 2. 创建一个自己的 plugin

https://bundler.io/v2.0/guides/bundler_plugins.html#getting-started-with-development

比如实现 **拦截 bundle install** 的过程:

```ruby
Bundler::Plugin.add_hook('before-install-all') do |dependencies|
  # Do something with the dependencies
end
```



## 9. can't find gem bundler (>= 0.a) with executable bundle (Gem::GemNotFoundException) rake aborted!

### 1. install ruby-gem 报错如下

```ruby
Traceback (most recent call last):
	4: from /Users/Fuhanyu/.rvm/gems/ruby-2.5.1/bin/ruby_executable_hooks:24:in `<main>'
	3: from /Users/Fuhanyu/.rvm/gems/ruby-2.5.1/bin/ruby_executable_hooks:24:in `eval'
	2: from /Users/Fuhanyu/.rvm/gems/ruby-2.5.1/bin/bundle:23:in `<main>'
	1: from /Users/Fuhanyu/.rvm/rubies/ruby-2.5.1/lib/ruby/2.5.0/rubygems.rb:308:in `activate_bin_path'
/Users/Fuhanyu/.rvm/rubies/ruby-2.5.1/lib/ruby/2.5.0/rubygems.rb:289:in `find_spec_for_exe': can't find gem bundler (>= 0.a) with executable bundle (Gem::GemNotFoundException)
Traceback (most recent call last):
	4: from /Users/Fuhanyu/.rvm/gems/ruby-2.5.1/bin/ruby_executable_hooks:24:in `<main>'
	3: from /Users/Fuhanyu/.rvm/gems/ruby-2.5.1/bin/ruby_executable_hooks:24:in `eval'
	2: from /Users/Fuhanyu/.rvm/gems/ruby-2.5.1/bin/bundle:23:in `<main>'
	1: from /Users/Fuhanyu/.rvm/rubies/ruby-2.5.1/lib/ruby/2.5.0/rubygems.rb:308:in `activate_bin_path'
/Users/Fuhanyu/.rvm/rubies/ruby-2.5.1/lib/ruby/2.5.0/rubygems.rb:289:in `find_spec_for_exe': can't find gem bundler (>= 0.a) with executable bundle (Gem::GemNotFoundException)
rake aborted!
LoadError: cannot load such file -- bundler/gem_tasks
/private/tmp/kanshan/Rakefile:1:in `<top (required)>'
/Users/Fuhanyu/.rvm/gems/ruby-2.5.1/gems/rake-12.3.1/exe/rake:27:in `<top (required)>'
/Users/Fuhanyu/.rvm/gems/ruby-2.5.1/bin/ruby_executable_hooks:24:in `eval'
/Users/Fuhanyu/.rvm/gems/ruby-2.5.1/bin/ruby_executable_hooks:24:in `<main>'
(See full trace by running task with --trace)
```

### 2. 关键报错

```ruby
can't find gem bundler (>= 0.a) with executable bundle (Gem::GemNotFoundException)
rake aborted!
```

看起来好像是与 **bundler 版本** 相关的问题.

### 3. 对比 bundler 版本

#### 1. 报错的机器上的 bundler 版本: 2.0.2

```
 ~  bundle -v
Bundler version 2.0.2
```

#### 2. ruby-gem 使用的 bundler 版本: 2.0.1

```ruby
# Gemfile.lock

.......

DEPENDENCIES
  bundler (~> 2.0.1)
  kanshan!
  rake (~> 10.0)
  rspec (~> 3.0)
  rubocop (= 0.59.0)
  simplecov
  simplecov-html

BUNDLED WITH
   2.0.1
```

发现这两个是不一致的.

### 4. 尝试让 本机 保持与 ruby-gem 一样的 bundler 版本 2.0.1

```
gem uninstall bundler
gem install bundler -v 2.0.1
```

再次安装 ruby-gem 就 ok 了..