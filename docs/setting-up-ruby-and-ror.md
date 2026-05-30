### 1. Install System Dependencies

```bash
sudo dnf install git-core zlib zlib-devel gcc-c++ patch readline readline-devel \
libffi-devel openssl-devel make bzip2 autoconf automake libtool bison \
sqlite-devel postgresql-devel libyaml-devel ruby-devel
```

### 2. Install a Ruby Version Manager

1. **Clone rbenv:** `git clone https://github.com/rbenv/rbenv.git ~/.rbenv`
2. **Configure your shell:** `echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc`.
3. **Install ruby-build:** This is a plugin that actually installs Ruby versions.
   `git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build`

### 3. Install Ruby

```bash
rbenv install 3.3.x  # Install the latest stable 3.3 release
rbenv global 3.3.x   # Set this as your default version
ruby -v              # Verify the version
```

### 4. Install Rails

Once Ruby is set up, you can install the Rails gem. As of 2026, you should aim for **Rails 8.1.0 or newer**.

```bash
gem install rails -v 8.1.0
rails -v  # Verify installation
```
