# Ubuntu install instructions

For the first portion of the class, we'll be working exclusively inside of the browser and Node. We'll be installing the following tools.

* Slack
* Git
* Node
* Postgres
* Ruby
* Rails

**TIP:** Use `CTRL+SHIFT+V` to paste into terminal

## Slack

We will be using slack to communicate throughout the course. You should've received an invite to our channels via e-mail. You can login via the web browser, but downloading / installing the app is highly recommended.

[Download Slack](https://slack.com/downloads)

## GIT
Before we do this process, please make sure you have signed up for an account on [Github](http://www.github.com). We will be installing a version of GIT from home brew and also configuring it.

To install GIT
```
sudo apt-get install git-all
```

#### Configuring GIT

Using your email credentials for GIT, run these commands with your user and email configured.

```
git config --global user.name "YOUR-USERNAME"
git config --global user.email "YOUR-EMAIL-ADDRESS"
git config --global push.default simple
git config --global credential.helper cache
```

### Setting up the bash shell
> do you have any other shell configuration files in your home directory?
> `ls -la ~`
> If you have something named `.bashrc`, `.bashrc`, `.bash_profile`
> Take the contents out of this file and put it in the new one we are creating. Then delete the old file.

Create a new shell config file.
```
touch ~/.bashrc
```

## Sublime
We'll be running **Sublime**, (not Sublime 2) as our text editor of choice.

Install via the package manager

```
sudo add-apt-repository ppa:webupd8team/sublime-text-3
sudo apt-get update
sudo apt-get install sublime-text-installer
```

If the above does not work, try installing via Sublime's website: [http://www.sublimetext.com/3](http://www.sublimetext.com/3) Download the `.deb` file and run it to install.

#### set sublime as your command line default editor
**this also allows you to refresh changes to your shell config**
```
export VISUAL=sublime
export EDITOR="$VISUAL"

function subledit() {
  subl ~/.bashrc
}

function profilerefresh() {
  echo "Refreshing your configuration."
  source ~/.bashrc
}
```

### Install the sublime `packagecontrol` library
Package Control allows you to add new functionality to sublime.

[https://packagecontrol.io/installation](https://packagecontrol.io/installation)

#### Using Package Control
[https://packagecontrol.io/docs/usage](https://packagecontrol.io/docs/usage)

> Package Control is driven by the Command Palette. To open the palette, press `ctrl+shift+p` (Win, Linux) or `cmd+shift+p` (OS X). All Package Control commands begin with Package Control:, so start by typing Package.

### Set Sublime to use `editorconfig` files
- `cmd+shift+p` type in `Package Control: Install Package` (auto-complete will help you) and press return
- type in `editorconfig` to install the package

### Get the `.editorconfig` file
You can find this file in your class repo. Copy it into your home directory.
```
cp /path/to/file ~/.
```

### Set Sublime to run as your git commit message editor
```
git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -w"
```

## Node

- Run the following commands in order in the terminal. Wait for each to complete before running the next.
	- `sudo apt-get update && sudo apt-get upgrade`
	- `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`
	- `sudo apt-get install -y nodejs`
  - `mkdir ~/.npm`
  - `npm config set prefix '~/.npm'`
  - `echo 'export PATH="$HOME/.npm/bin:$PATH"' >> ~/.bashrc`
  - `source ~/.bashrc`
	- `node -v` (Checks the installed version of NodeJS)
	- `npm -v` (Checks the installed version of npm)


To finish up your installation, run this command to allow for global installations of npm tools.

```
sudo chown -R $USER /usr/local/lib
```



## Postgres

## Installing PostgreSQL
- Any database system contains at least 2 parts: the database server itself, and the database client.

- __Installing the PostgreSQL client__
- Open a WSL terminal and run the following commands in order.
- `sudo apt-get install postgresql-client postgresql postgresql-contrib postgresql-client-common`



### Configure Postgres User

You'll also need to configure a user for your Postgres database.

```
sudo -u postgres psql postgres

\password postgres

```

Choose an easy to remember password then type `\quit` to exit psql. MAKE SURE YOU REMEMBER THIS PASSWORD YOU WILL NEED IT LATER.


### Create a Postgres Alias

To make it easier to start postgres we're going to create a couple aliases. Edit your bashrc file by typing `subl ~/.bashrc` add these lines to the bottom of the file:

```
alias psql="sudo -u postgres psql"
alias pgserver="sudo -u postgres service postgresql start"
```

**pgserver** will be used to start the postgres server

**psql** will be used to access the psql termainal

### Testing Postgres Setup

Quit terminal and reopen it before testing.

**Start Server**

```
pgserver
```

**enter psql terminal**

```
psql
```

Should enter psql terminal and have no error.

**exit psql**

```
\q
```

### Passwordless Postgres

Set no-password on postgres for your computer, so that it's easier to work with.


Edit postgres configuration file:

```
sudo sublime /etc/postgresql/POSTGRE_VERSION/main/pg_hba.conf
```

The file will look like this:

Change all configuration access to:

```
 # Database administrative login by Unix domain socket
 local   all             all                                     trust

 # TYPE  DATABASE        USER            ADDRESS                 METHOD

 # "local" is for Unix domain socket connections only
 local   all             all                                     trust
 # IPv4 local connections:
 host    all             all             127.0.0.1/32            trust
 # IPv6 local connections:
 host    all             all             ::1/128                 trust

```
Restart postgres server

```

 sudo /etc/init.d/postgresql restart

```

##Installing Ruby on Rails

####Install dependencies

```
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
```

###Install rbenv / ruby

rbenv lets us change ruby verions on the fly, useful for working with diffrent rails apps.

```
cd ~
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

sudo chown -R $USER ~/.rbenv

rbenv install 2.5.1
```

(last step above will take a LONG time)

**Set ruby version and check that it worked**

```
rbenv global 2.5.1
ruby -v
```

####Install Rails

Before moving on close and reopen terminal.

```
gem update --system
echo "gem: --no-ri --no-rdoc" > ~/.gemrc
gem install bundler
gem install rails
```

(the last step will take a while)


#####Verify your installation

Run each of these commands and then call someone over to validate your installation is correct.

```
rails -v
ruby -v

which ruby
which rails
which bundle
which gem

node -v
npm -v

git --version
psql --version
subl -v

```

## Setting up a server

Occasionally you'll encounter permission errors when running websites using the file protocol, for example accessing loading local JSON files to our page. To solve this you'll need to run a HTTP server. If you're using BrowserSync, you won't need to worry about this, alternatively you could build a quick Node static server. An easier option however, is to use the local python server.

We'll be setting up a command line alias to start a Python server.

1.) edit your `.bashrc`

```
subl ~/.bashrc
```

2.) Insert this code near the bottom of the file:

```
alias srv="_srv(){xdg-open \"http://localhost:\${1-8000}\" && python -m SimpleHTTPServer \$1}; _srv"
```

3.) Close and restart your terminal.

Now you should be able to navigate to the folder of your project (the folder containing index.html), type `srv`, and hit enter. This will start a HTTP server and open your browser to that URL.

You can go back to the site at anytime by going to `http://localhost:8000`. You can quit the server by typing `CTRL + C`


#### Aside: Wildcard Protocols and HTTP

You'll notice that Bootstrap and other CDN URLs may start with `//`. This is a wildcard protocol, which means it will use whatever protocol your site is using (`http://` or `https://`). When we're loading a file locally, our protocol is `file:///`, meaning we're accessing a file on our harddrive. Therefore, the default CDN will look for the file on our computer (instead of on the CDN) and won't find it. To fix this, we need to run a HTTP server.
