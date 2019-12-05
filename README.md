# 🔀 switch-php

Easily switch between PHP versions on your Mac. Requires Homebrew and works with Laravel Valet.

![switch-php screencast](./switch-php.gif)


### ⬇️️️ Installation:

Installing `switch-php` is as easy as running:

```
npm install --global switch-php
```

If you use Yarn, you can do this:

```
yarn global add switch-php
```

Alternatively you can move the `switch-php.sh` file from this repo into your home directory and add this line in your `.bashrc`/`.zshrc`/etc.:

```
source ~/switch-php.sh
```


### ⚙ Usage:

You must have PHP installed via Homebrew in order for `switch-php` to work. `switch-php` also works really well with Laravel Valet, but Valet is not a requirement in order to use `switch-php`.

Here's an example of how you would use `switch-php`:

```
$ switch-php 7.1 -v -m 512M
```

1. `switch-php` -> The main command. (Required)
1. `7.1` -> Specify the version of PHP you want to switch too, in this case `php71`. (Required)
1. `-v` -> Request verbose output (Optional)
1. `-m 512M` -> Request a custom PHP memory setting. If you don't pass an additional arugment, the memory will be reset to the Valet default. (Optional)

Here's the full list of versions/options:

```bash
Usage:
  version [options] [arguments]

Options:
  -h, --help      Display the help message
  -v, --verbose   Display more info during the process
  -m, --memory    Customize the PHP memory setting (Valet only)

Available Versions:
  5.6              Switch to php@5.6
  7.0              Switch to php@7.0
  7.1              Switch to php@7.1
  7.2              Switch to php@7.2
  7.3              Switch to php@7.3
  7.4		   Switch to php@7.4
```


### 🎛 Customizing the PHP Memory Settings:

- If you don't pass an argument to `-m` or `--memory`, it will reset any previously set custom memory settings to the default Valet config.
- Alternatively, you can pass an argument to `-m` or    `--memory` if you want to override the default Valet memory settings. For example, you can do:

```
switch-php 7.1 -m 512M       # php@7.1 with 512MB of memory
switch-php 7.3 -m 2G -v      # php@7.3 with 2GB of memory; verbose output
switch-php 5.6 --memory=1G   # php@5.6 with 1GB of memory
```

- *Note: customizing PHP memory settings currently only works for Laravel Valet users. If you don't use Valet, we hope to get this working for you as well in an upcoming release.*
