[TOC]



## 1. pod install

### 1. Podfile

```ruby
# Uncomment the next line to define a global platform for your project
platform :ios, '10.0'

target 'DemoApp' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!
  pod 'AFNetworking'
end
```

### 2. output

```
 ~/Desktop/DemoApp  pod install
Analyzing dependencies
Downloading dependencies
Using AFNetworking (3.2.1)
Generating Pods project
Integrating client project
Sending stats
Pod installation complete! There is 1 dependency from the Podfile and 1 total pod installed.
```



## 2. pod install(command lint) => cocoapods-1.6.1/lib/cocoapods/command/install.rb

### 1. lib/cocoapods/command/install.rb

```ruby
module Pod
  class Command
    class Install < Command
      include RepoUpdate
      include ProjectDirectory

      self.summary = 'Install project dependencies according to versions from a Podfile.lock'

      self.description = <<-DESC
        ...........
      DESC

      def self.options
        [
          ['--repo-update', 'Force running `pod repo update` before install'],
          ['--deployment', 'Disallow any changes to the Podfile or the Podfile.lock during installation'],
        ].concat(super).reject { |(name, _)| name == '--no-repo-update' }
      end

      def initialize(argv)
        super
        @deployment = argv.flag?('deployment', false)
      end

      def run
        verify_podfile_exists!
        installer = installer_for_config
        installer.repo_update = repo_update?(:default => false)
        installer.update = false
        installer.deployment = @deployment
        installer.install!
      end
    end
  end
end
```

### 2. lib/cocoapods/command.rb 定义 verify_podfile_exists!

```ruby
module Pod
  class PlainInformative
    include CLAide::InformativeError
  end

  class Command < CLAide::Command
    require 'cocoapods/command/options/repo_update'
    require 'cocoapods/command/options/project_directory'
    ........

    def verify_podfile_exists!
      unless config.podfile
        raise Informative, "No `Podfile' found in the project directory."
      end
    end

    ........
  end
end
```

### 3. 打印 run 方法中的 installer 对象

```ruby
def run
  verify_podfile_exists! #=> Pod::Command#verify_podfile_exists!
  installer = installer_for_config
  puts '-' * 50
  pp installer
  puts '-' * 50
  pp installer.class
  installer.repo_update = repo_update?(:default => false)
  installer.update = false
  installer.deployment = @deployment
  # installer.install!
end
end
```

```
 ~/Desktop/DemoApp  pod install
--------------------------------------------------
#<Pod::Installer:0x00007fc025a495d8
 @has_dependencies=true,
 @lockfile=#<Pod::Lockfile>,
 @pod_installers=[],
 @podfile=
  #<Pod::Podfile:0x00007fc025a16840
   @current_target_definition=#<Pod::Podfile::TargetDefinition label=Pods>,
   @defined_in_file=#<Pathname:/Users/xiongzenghui/Desktop/DemoApp/Podfile>,
   @internal_hash={},
   @root_target_definitions=[#<Pod::Podfile::TargetDefinition label=Pods>]>,
 @sandbox=#<Pod::Sandbox> with root /Users/xiongzenghui/Desktop/DemoApp/Pods,
 @use_default_plugins=true>
--------------------------------------------------
Pod::Installer
```

使用的是 **Pod::Installer** 类的对象.



## 3. lib/cocoapods/installer.rb

### 0. Pod::Installer

```ruby
class Installer
  autoload :Analyzer,                   'cocoapods/installer/analyzer'
  autoload :InstallationOptions,        'cocoapods/installer/installation_options'
  autoload :PostInstallHooksContext,    'cocoapods/installer/post_install_hooks_context'
  autoload :PreInstallHooksContext,     'cocoapods/installer/pre_install_hooks_context'
  autoload :SourceProviderHooksContext, 'cocoapods/installer/source_provider_hooks_context'
  autoload :PodfileValidator,           'cocoapods/installer/podfile_validator'
  autoload :PodSourceInstaller,         'cocoapods/installer/pod_source_installer'
  autoload :PodSourcePreparer,          'cocoapods/installer/pod_source_preparer'
  autoload :UserProjectIntegrator,      'cocoapods/installer/user_project_integrator'
  autoload :Xcode,                      'cocoapods/installer/xcode'
  ..........

  def initialize(sandbox, podfile, lockfile = nil)
    @sandbox  = sandbox || raise(ArgumentError, 'Missing required argument `sandbox`')
    @podfile  = podfile || raise(ArgumentError, 'Missing required argument `podfile`')
    @lockfile = lockfile

    @use_default_plugins = true
    @has_dependencies = true
    @pod_installers = []
  end


  # pod install 调用到的方法
  def install!
    prepare # 1.
    resolve_dependencies # 2.
    download_dependencies # 3.
    validate_targets # 4.
    generate_pods_project # 5.
    if installation_options.integrate_targets?
      integrate_user_project # 6. 
    else
      UI.section 'Skipping User Project Integration'
    end
    perform_post_install_actions # 7.
  end
end
```

### 1. Pod::Installer#prepare

```ruby
def prepare
  # 1. Raise if pwd is inside Pods
  if Dir.pwd.start_with?(sandbox.root.to_path)
    message = 'Command should be run from a directory outside Pods directory.'
    message << "\n\n\tCurrent directory is #{UI.path(Pathname.pwd)}\n"
    raise Informative, message
  end

  # 2. 
  UI.message 'Preparing' do
    deintegrate_if_different_major_version #=> 1. ococpoads majer version check
    sandbox.prepare #=> 2. Pods 目录结构 初始化
    ensure_plugins_are_installed! #=> 3. plugins installed
    run_plugins_pre_install_hooks #=> 4. 执行所有的 pre_install() 注册的方法
  end
end
```

### 2. Pod::Installer#resolve_dependencies

```ruby

```

### 3. Pod::Installer#download_dependencies

```ruby

```

### 4. Pod::Installer#validate_targets

```ruby

```

### 5. Pod::Installer#generate_pods_project

```ruby

```

### 6. Pod::Installer#integrate_user_project

```ruby

```

### 7. Pod::Installer#perform_post_install_actions

```ruby

```