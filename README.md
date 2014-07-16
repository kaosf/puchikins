# Puchikins

"Puchi" means "Mini" in Japanese.

This is a mini Jenkins link application which realize some features of observing
a simple trigger, running a shellscript for tests and sending an E-mail.

## Prerequisites

* Bash
* Make
* Sendmail

### Recommended

* Git

## Installation

```bash
./configure
# or
#./configure --prefix=$HOME/local

make

sudo make install
# or
#make install
```

## Usage example

```bash
# Initialize remote repository and setup post-receive hook
git init some/where/my-awesome-project --bare
cp post-receive.sample some/where/my-awesome-project/hooks/post-receive

# Setup puchikins working directory
cd any/where
git clone some/where/my-awesome-project
cd my-awesome-project

# Create build commands script
cat <<EOF > ../command.sh
git fetch origin dev
git checkout FETCH_HEAD
echo Done tests
exit 0
EOF

# Start Puchikins service
puchikinsd ../commands.sh noreply@my.domain myname@my.domain
# Ctrl-C to stop the service

# Start your working!
cd your/working/dir
git clone some/where/my-awesome-project
cd my-awesome-project

# Edit my-awesome-project and commit

git push origin master
```

After pushing, the remote repository create `/tmp/puchikins-trigger`. The
Puchikins service detects it and run `../commands.sh`. If it returns non zero
value, Puchikins send an E-mail which includes the STDOUT and STDERR of the
command to `myname@my.domain` from `noreply@my.domain` by `sendmail` command.

So you may create `commands.sh` as to return either zero or non zero value
depending on the test result.

## TODO

* More dog fooding (At least until 2014-08-31)
* Enable to run 2 Puchikins services
  * Resolve `/tmp/puchikins-trigger` conflicting

## License

[MIT](http://opensource.org/licenses/MIT)

Copyright (C) 2014 ka
