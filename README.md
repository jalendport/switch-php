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
  7.4              Switch to php@7.4
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



### 🎁 Extend with Pre and Post functions

Optionally extend switch_php with pre and post function calls - support your own custom ad-hoc tasks (in the ever changing brew world!).

You may define `_switch_php_pre_tasks` and `_switch_php_post_tasks` that wrap the default switch-php behaviour, both functions take 2 arguments, `phpversion` and `verbose` .



Example usage 

>  prior to switching ensure php@7.0 has the required formula/Dyld

````
# add these functions to your ~/.bash_profile


function _switch_php_pre_tasks() {
    [ $# -lt 1 ] && return 1
    phpver=$1
    verbose="${2:-0}"

    # DO YOUR CUSTOM TASKS
    # my php@70 is always breaking - try this 
    if [ "${phpver}" = 'php@7.0' ]; then
        [ "${verbose}" -eq 1 ] && echo " 🚩  Dyld fixing at (icu4c 64.2, openssl 1.0.2) for ${phpver}.";
        brew switch icu4c 64.2 &>/dev/null || echo " 🚩 - icu4c 64.2 missing - brew reinstall https://raw.githubusercontent.com/Homebrew/homebrew-core/a806a621ed3722fb580a58000fb274a2f2d86a6d/Formula/icu4c.rb"
        brew switch openssl 1.0.2t &>/dev/null || echo " 🚩 - openssl 1.0.2 missing - brew reinstall brew reinstall https://raw.githubusercontent.com/Homebrew/homebrew-core/8b9d6d688f483a0f33fcfc93d433de501b9c3513/Formula/openssl.rb"
    else
        [ "${verbose}" -eq 1 ] && echo " 🚩  Dyld reverting to (icu4c 67.1) for ${phpver}.";
        brew switch icu4c 67.1 &>/dev/null || echo " 🚩 - icu4c 67.1 missing - brew install icu4c -v"
    fi

}

function _switch_php_post_tasks() {
    [ $# -lt 1 ] && return 1
    phpver=$1
    verbose="${2:-0}"

    # DO YOUR CUSTOM TASKS
    [ "${verbose}" -eq 1 ] && echo "No action in custom post tasks" 
}


# important to export the functions otherwise switch-php will not be able to call it.
export -f _switch_php_pre_tasks
export -f _switch_php_post_tasks
````

These two functions will now be called each time switch-php is run.
```
switch-php 7.0 -v
 👀  Verifying that Valet is installed...
Password:
 🔍  Checking which PHP versions are installed...
 🚩  Dyld fixing at (icu4c 64.2, openssl 1.0.2) for php@7.0.
 🛑  Stopping Valet...
 ==>  Stopping nginx...
 ✅  Valet stopped
 🔀  Switching to php@7.0...
 ==>  Stopping php@7.0...
 ==>  Unlinking php@7.0...
 ==>  Stopping php@7.2...
 ==>  Unlinking php@7.2...
 ==>  Stopping php@7.4...
 ==>  Unlinking php@7.4...
 ==>  Linking php@7.0...
 ==>  Starting php@7.0...
 ✅  PHP switched
 ⚙  Starting Valet...
 ==>  Starting nginx...
 ✅  Valet started

You are now using PHP 7.0.33

No action in custom post tasks
```
