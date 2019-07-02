[TOC]



## 1. cocoapods.gemspec  (类似于 AFNetworking.podspec ==组件描述== 配置文件)

### 1. cocoapods.gemspec 源码

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
  s.add_runtime_dependency 'xcodeproj',             '>= 1.8.2', '< 2.0'

  ## Version 5 needs Ruby 2.2, so we specify an upper bound to stay compatible with system ruby
  s.add_runtime_dependency 'activesupport', '>= 4.0.2', '< 5'
  s.add_runtime_dependency 'colored2',       '~> 3.1'
  s.add_runtime_dependency 'escape',        '~> 0.0.4'
  s.add_runtime_dependency 'fourflusher',   '>= 2.2.0', '< 3.0'
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

### 2. s.files 指定 gem ruby 所有源文件

```ruby
s.files = Dir["lib/**/*.rb"] + %w{ bin/pod bin/sandbox-pod README.md LICENSE CHANGELOG.md }
```

### 3. dependency

#### 1. CocoaPods-Core gem

```ruby
# Link with the version of CocoaPods-Core
s.add_runtime_dependency 'cocoapods-core',        "= #{Pod::VERSION}"
s.add_runtime_dependency 'cocoapods-deintegrate', '>= 1.0.3', '< 2.0'
s.add_runtime_dependency 'cocoapods-downloader',  '>= 1.2.2', '< 2.0'
s.add_runtime_dependency 'cocoapods-plugins',     '>= 1.0.0', '< 2.0'
s.add_runtime_dependency 'cocoapods-search',      '>= 1.0.0', '< 2.0'
s.add_runtime_dependency 'cocoapods-stats',       '>= 1.0.0', '< 2.0'
s.add_runtime_dependency 'cocoapods-trunk',       '>= 1.3.1', '< 2.0'
s.add_runtime_dependency 'cocoapods-try',         '>= 1.1.0', '< 2.0'
```

#### 2. 较重要的 Cocoapods 其他 gem

```ruby
s.add_runtime_dependency 'claide',                '>= 1.0.2', '< 2.0'
s.add_runtime_dependency 'molinillo',             '~> 0.6.6'
s.add_runtime_dependency 'xcodeproj',             '>= 1.8.2', '< 2.0'
```

#### 3. 其他的 第三方 依赖 gem

```ruby
## Version 5 needs Ruby 2.2, so we specify an upper bound to stay compatible with system ruby
s.add_runtime_dependency 'activesupport', '>= 4.0.2', '< 5'
s.add_runtime_dependency 'colored2',       '~> 3.1'
s.add_runtime_dependency 'escape',        '~> 0.0.4'
s.add_runtime_dependency 'fourflusher',   '>= 2.2.0', '< 3.0'
s.add_runtime_dependency 'gh_inspector',  '~> 1.0'
s.add_runtime_dependency 'nap',           '~> 1.0'
s.add_runtime_dependency 'ruby-macho',    '~> 1.4'
```

### 4. development dependency

```ruby
s.add_development_dependency 'bacon', '~> 1.1'
s.add_development_dependency 'bundler', '~> 1.3'
s.add_development_dependency 'rake', '~> 10.0'
```

### 5. rubygems version = "1.6.2"

```ruby
s.rubygems_version = "1.6.2"
s.required_rubygems_version = Gem::Requirement.new(">= 0") if s.respond_to? :required_rubygems_version=
```

### 6. ruby version = '>= 2.0.0'

```ruby
s.required_ruby_version = '>= 2.0.0'
```


## 2. Gemfile (类似于 Podfile ==下载安装组件== 的配置文件)





## 3. cocoapods 包含的 Projects

CocoaPods is composed of the following projects:

| Status                                                       | Project                                                      | Description                                               | Info                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| [![Build Status](https://camo.githubusercontent.com/dfeb5d3186a7fd518e57e5279f2714b883091ad5/687474703a2f2f696d672e736869656c64732e696f2f7472617669732f436f636f61506f64732f436f636f61506f64732f6d61737465722e7376673f7374796c653d666c6174)](https://travis-ci.org/CocoaPods/CocoaPods) | [CocoaPods](https://github.com/CocoaPods/CocoaPods)          | The CocoaPods command line tool.                          | [guides](https://guides.cocoapods.org/)                      |
| [![Build Status](https://camo.githubusercontent.com/c264b470965d85d43b8794ae695985425849977b/687474703a2f2f696d672e736869656c64732e696f2f7472617669732f436f636f61506f64732f436f72652f6d61737465722e7376673f7374796c653d666c6174)](https://travis-ci.org/CocoaPods/Core) | [CocoaPods Core](https://github.com/CocoaPods/Core)          | Support for working with specifications and podfiles.     | [docs](https://guides.cocoapods.org/contributing/components.html) |
| [![Build Status](https://camo.githubusercontent.com/596cdc908123b1df24d89ea9c792067ef0861ec2/687474703a2f2f696d672e736869656c64732e696f2f7472617669732f436f636f61506f64732f636f636f61706f64732d646f776e6c6f616465722f6d61737465722e7376673f7374796c653d666c6174)](https://travis-ci.org/CocoaPods/cocoapods-downloader) | [CocoaPods Downloader](https://github.com/CocoaPods/cocoapods-downloader) | Downloaders for various source types.                     | [docs](https://www.rubydoc.info/gems/cocoapods-downloader)   |
| [![Build Status](https://camo.githubusercontent.com/f2b78c75c27a98a6c9d626b3ff921d501faec88b/687474703a2f2f696d672e736869656c64732e696f2f7472617669732f436f636f61506f64732f58636f646570726f6a2f6d61737465722e7376673f7374796c653d666c6174)](https://travis-ci.org/CocoaPods/Xcodeproj) | [Xcodeproj](https://github.com/CocoaPods/Xcodeproj)          | Create and modify Xcode projects from Ruby.               | [docs](https://www.rubydoc.info/gems/xcodeproj)              |
| [![Build Status](https://camo.githubusercontent.com/f6098bc5922b8b855794c68cf6fb1359634065f1/687474703a2f2f696d672e736869656c64732e696f2f7472617669732f436f636f61506f64732f434c416964652f6d61737465722e7376673f7374796c653d666c6174)](https://travis-ci.org/CocoaPods/CLAide) | [CLAide](https://github.com/CocoaPods/CLAide)                | A small command-line interface framework.                 | [docs](https://www.rubydoc.info/gems/claide)                 |
| [![Build Status](https://camo.githubusercontent.com/be9619de10eb29eb627ca1167d64bb179d868b52/687474703a2f2f696d672e736869656c64732e696f2f7472617669732f436f636f61506f64732f4d6f6c696e696c6c6f2f6d61737465722e7376673f7374796c653d666c6174)](https://travis-ci.org/CocoaPods/Molinillo) | [Molinillo](https://github.com/CocoaPods/Molinillo)          | A powerful generic dependency resolver.                   | [docs](https://www.rubydoc.info/gems/molinillo)              |
| [![Build Status](https://camo.githubusercontent.com/45c036a9f96db7b7f2e744ab44b81878b2204bec/687474703a2f2f696d672e736869656c64732e696f2f7472617669732f436f636f61506f64732f436f636f61506f64732d6170702f6d61737465722e7376673f7374796c653d666c6174)](https://travis-ci.org/CocoaPods/CocoaPods-app) | [CocoaPods.app](https://github.com/CocoaPods/CocoaPods-app)  | A full-featured and standalone installation of CocoaPods. | [info](https://cocoapods.org/app)                            |
|                                                              | [Master Repo](https://github.com/CocoaPods/Specs)            | Master repository of specifications.                      | [guides](https://guides.cocoapods.org/making/specs-and-specs-repo.html) |

