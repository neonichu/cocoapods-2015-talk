# CocoaPods 💥

## Capital One, September 2015

### Boris Bügling - @NeoNacho

### Samuel Giddins - @segiddins

![](images/cocoapods.jpg)

<!--- use Next theme, white -->

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

# Dealing With 🐛s

![](images/cocoapods.jpg)

---

- Does it happen only in my project?
- Can I make it happen without custom settings?
- Work around it via a post-install script
- Submit an issue

![](images/cocoapods.jpg)

---

## Submitting an Issue

- Following the contributing guidelines
- State which of the above you've done
- Make sure you're not leaving out a key piece of info
- Don't file dupes, don't 👍

![](images/cocoapods.jpg)

---

The easier you make it for us to fix a bug, the faster it will get fixed.

![](images/cocoapods.jpg)

---

# Road to 1.0

![](images/cocoapods.jpg)

---

_Long_ road to 1.0

![](images/cocoapods.jpg)

---

CocoaPods == Stable

![](images/cocoapods.jpg)

---

CocoaPods == Stable __*__

__*__ For 99% of users

![](images/cocoapods.jpg)

---

_2_ major changes

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

# 1.0

![](images/cocoapods.jpg)

---

# Our promise we won't intentionally break things

![](images/cocoapods.jpg)

---

## Coming this fall

![](images/cocoapods.jpg)

---

## Coming this fall

Hopefully

![](images/cocoapods.jpg)

---

# Thank you!

![](images/thanks.gif)

---

## @NeoNacho

## @segiddins

### cocoapods.org

![](images/cocoapods.jpg)
