# CocoaPods 💥

## iOSDevCampDC, September 2015

### Boris Bügling - @NeoNacho

![20%, original, inline](images/contentful.png)

![](images/cocoapods.jpg)

<!--- use Next theme, white -->

---

## CocoaPods

![](images/cocoapods.jpg)

---

## Contentful

![](images/contentful-bg.png)

---

# Agenda

- What's new and upcoming
- CocoaPods plugins
- How does CocoaPods even
- Troubleshooting and Tips

![](images/cocoapods.jpg)

---

# What's new and upcoming

![inline](images/doge.gif)

![](images/cocoapods.jpg)

---

# CocoaPods 0.38

![](images/cocoapods.jpg)

---

# support for watchOS

```ruby
Pod::Spec.new do |s|
  # …
  s.watchos.deployment_target = '2.0'
end
```
![](images/cocoapods.jpg)

---

# Target Deduplication

Happens by default, exceptions:

* They are used on different platforms.
* They are used with differents sets of subspecs.
* They have any dependency which needs to be duplicated.

![](images/cocoapods.jpg)

---

# Breaking Change to the Hooks-API

```ruby
post_install do |installer_or_rep|
  # @note: Remove after CocoaPods 0.38+ is completely rolled out
  # and rename block argument to `installer`.
  installer = installer_or_rep.respond_to?(:installer) ? 
    installer_or_rep.installer : installer_or_rep

  # You can use the installer from now on.
end
```

![](images/cocoapods.jpg)

---

# Split of `xcconfig`

- `pod_target_xcconfig`
- `user_target_xcconfig`

![](images/cocoapods.jpg)

---

# Other changes

- Removal of the Enviroment Header
- Deterministic UUIDs by Xcodeproj
- Resolver takes the Deployment Target into account

![](images/cocoapods.jpg)

---

## Make sure you check out `blog.cocoapods.org` and the `CHANGELOG` for major releases

![](images/cocoapods.jpg)

---

# CocoaPods 0.39

- currently beta 4
- Vendored dynamic frameworks 🎉
- `--private` option for linting (ignore warnings only suitable for trunk)
- `HEADER_SEARCH_PATHS` is no longer constructed recursively

![](images/cocoapods.jpg)

---

# Road to 1.0

![](images/cocoapods.jpg)

---

## Major Changes

# Customizable Installation

![](images/cocoapods.jpg)

---

## Customizable Installation

- ~~`--no-integrate`~~
- Same default
- Pass options to the installer

![](images/cocoapods.jpg)

---

## Major Changes

# Target Inheritance

![](images/cocoapods.jpg)

---

## Target Inheritance

- ~~`:exclusive => true`~~
- New options
    - Complete
    - Search paths
    - None

![](images/cocoapods.jpg)

---

## Target Inheritance

- Allows test targets to work
- Allows for more 'logical' Podfiles

![](images/cocoapods.jpg)

---

## Coming this fall

Hopefully

![](images/cocoapods.jpg)

---

# CocoaPods plugins

![](images/cocoapods.jpg)

---

# CocoaPods plugins

- Add subcommands to `pod`, the tool
- `post_install` hook
- Each plugin is a Gem

![](images/cocoapods.jpg)

---

## Do whatever you want, because Ruby 💎

![](images/mind_blown.gif)

---

# Useful plugins

![](images/tools.jpg)

---

```bash
$ pod plugins list
```

![](images/inception.jpg)

---

```bash 
$ pod keys set AccessToken 0xFFFFFFFF
```

![](images/keys.jpg)

---

```bash
$ pod package ContentfulDeliveryAPI.podspec
```

![](images/package.jpg)

---

```bash
$ pod lib coverage
```

![](images/coverage.jpg)

---

```bash 
$ pod roulette
```

![](images/roulette.jpg)

--- 

# How to build your own plugin

![](images/Building-His-Church.jpg)

---

```bash
$ pod plugins create cocoapods-awesome-plugin
```

![](images/cocoapods.jpg)

---

```bash
$ tree
.
├── Gemfile
├── LICENSE.txt
├── README.md
├── Rakefile
├── cocoapods_awesome_plugin.gemspec
└── lib
    ├── cocoapods_awesome_plugin.rb
    ├── cocoapods_plugin.rb
    └── pod
        └── command
            └── plugin.rb

3 directories, 8 files
```

![](images/tree.jpg)

---

```ruby
module Pod
  class Command
    class Plugin < Command
      self.summary = "Short description."

      self.arguments = [CLAide::Argument.new('NAME', true)]

      def initialize(argv)
        @name = argv.shift_argument
        super
      end

      def validate!
        super
        help! "A Pod name is required." unless @name
      end

      def run
        UI.puts "Add your implementation here"
      end
```

![](images/cocoapods.jpg)

---

# Hooks

![](images/hook-1991.jpg)

---

```ruby
Pod::HooksManager.register(:post_install) do |options|
  require 'installer'

  UI.puts "This gets executed after installation"
end
```

![](images/cocoapods.jpg)

---

# How does CocoaPods even

![](images/work.gif)

---

# `use_frameworks!`

![](images/cocoapods.jpg)

---

# Build Phases

![inline](images/build_phases.png)

![](images/cocoapods.jpg)

---

# Check Pods Manifest.lock

```bash
diff "${PODS_ROOT}/../Podfile.lock" "${PODS_ROOT}/Manifest.lock" > /dev/null
if [[ $? != 0 ]] ; then
    cat << EOM
error: The sandbox is not in sync with the Podfile.lock. 
Run 'pod install' or update your CocoaPods installation.
EOM
    exit 1
fi
```

![](images/cocoapods.jpg)

---

# Link Binary With Libraries

- Links the aggregate framework
- Gets build via implicit dependencies

![](images/cocoapods.jpg)

---

# Radar 22392501

> Actual Results:
> It doesn't. You receive this error when archiving:
> [...]
> So, because they have the same product name, _they're overriding each other_.
> How are we supposed to use our own frameworks from our different target types?

![](images/cocoapods.jpg)

---

# Embed Pods Frameworks

- Copies the frameworks to the application bundle

![](images/cocoapods.jpg)

---

# Copy Pods Resources

- Copies resources to the application bundle

![](images/cocoapods.jpg)

---

# Troubleshooting

![](images/troubleshooting.gif)

---

# Mixing Objective-C and Swift in a Pod

- Frameworks can't have a separate bridging header
- Objective-C headers can be included in the umbrella header

![](images/cocoapods.jpg)

---

# First solution: module imports

```swift
import ObjectiveCPod
```

=> You get the full public API

![](images/cocoapods.jpg)

---

# Second solution: publiclize all the things

- Create a header
- Import all the Objective-C headers you need
- Add that header to `public_header_files`

=> Exposes the dependency in the public API

![](images/cocoapods.jpg)

---

# Third solution: custom module map

- Module maps can have private submodules

```
framework module ResearchKit {
    umbrella header "ResearchKit.h"
    
    module Private {
        umbrella header "ResearchKit_Private.h"
        export *
    }
    
    export *
    module * { export * }
}
```

- Can be declared in the podspec with `module_map`

![](images/cocoapods.jpg)

---

# Reset your integration

```bash
$ gem install cocoapods-deintegrate
$ pod deintegrate
$ pod install
```

![](images/cocoapods.jpg)

---

# `rm -rf $HOME/Library/Developer/Xcode/DerivedData`

![](images/cocoapods.jpg)

---

# Filing issues

- Read our contribution guidelines
- Try if your problem reproduces in a separate project
- Give us that project!

=> Help us help you

![](images/cocoapods.jpg)

---

# It gets *really* old creating countless projects every day :)

![](images/old.gif)

---

# Tips

![](images/cocoapods.jpg)

---

# Use bundler to pin your used CocoaPods version

```bash
$ cat Gemfile
source 'https://rubygems.org'

gem 'cocoapods', '= 0.36.0.beta.1'
$ bundle install
$ bundle exec pod install # from now on
```

![](images/cocoapods.jpg)

---

# Make sure your dependencies don't go away

- Commit the `Pods` directory
- Mirror your Pods (<https://github.com/xing/XNGPodsSynchronizer>)

![](images/cocoapods.jpg)

---

# You can keep the Pods history separate

- Create an orphaned branch on your repo
- Check it out as a submodule to `Pods`
- History is now completely separate

![](images/cocoapods.jpg)

---

# Automate your workflow

- Use plugins
- Use a Rakefile or Makefile for common tasks
- Use `pre_install` or `post_install` hooks in the Podfile

![](images/cocoapods.jpg)

---

# Thank you!

![](images/thanks.gif)

---

@NeoNacho

boris@contentful.com

http://buegling.com/talks

![](images/contentful-bg.png)
