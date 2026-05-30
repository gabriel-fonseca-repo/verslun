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

### 5. Create Your First Application

With Rails installed, you can generate a new application. Rails defaults to **SQLite** for development, but you can specify others with flags.

```bash
rails new my_app
cd my_app
```

Inside the directory, you will find a standardized structure including folders like `app/` (for your logic), `config/` (for settings), and `db/` (for your database schema).

### 6. Start the Development Server

Rails uses the **Puma** web server by default. Start it with:

```bash
bin/rails server
```

Navigate to `http://localhost:3000` in your browser. If you see the "Welcome" page, your environment is correctly configured.

### Recommended Tools for Fedora 44

- **Editor:** **Visual Studio Code**, **Zed**, or **RubyMine** are frequently used by Rubyists. VS Code and Zed support the Language Server Protocol (LSP) for advanced features like code completion.
- **Debugging:** The **Pry** gem is recommended as an alternative to the default IRB for a more powerful interactive shell.
- **Code Quality:** Use **RuboCop** to ensure your code follows established style guides like those from Shopify or Airbnb.

I have initiated a search for Fedora 44-specific package names to ensure the `dnf` commands are perfectly optimized for that version. You can check the sources panel for new results once the search completes.
