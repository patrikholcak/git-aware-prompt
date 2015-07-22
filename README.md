## Overview

If you `cd` to a Git working directory, you will see:
- the current Git branch name
- an information about how far ahead you are
- a star (`*`) indicating whether there are any uncommited changes

When you're not in a Git working directory, your prompt
works like normal.

![Git Branch in Prompt](https://raw.github.com/patrikholcak/git-aware-prompt/master/preview.png)


## Before installation
The performance of your prompt might (and most certainly will) be affected by this script. I did some tests and the script executes in under 30ms.


## Installation

Edit your `~/.bash_profile` or `~/.profile` and add the following to the top:

```bash
find_git_branch() {
  local branch
  if branch=$(git symbolic-ref --short HEAD 2> /dev/null); then

    if [[ "$branch" == "HEAD" ]]; then
      branch='detached*'
    fi

    local ahead=$(git rev-list @{u}.. --count 2> /dev/null)

    if [[ "$ahead" > "0" ]]; then
      branch="$branch[$ahead]"
    fi

    local status=$(git status --porcelain 2> /dev/null)

    if [[ "$status" != "" ]]; then
      branch="$branch*"
    fi

    git_branch="@$branch"
  else
    git_branch=""
  fi
}

PROMPT_COMMAND="find_git_branch; $PROMPT_COMMAND"
```


## Configuring

Once installed, there will be new `$git_branch` variable
available to use in the `PS1` environment variable.

If you want to know more about how to customize your prompt, I recommend
this article: [How to: Change / Setup bash custom prompt (PS1)][how-to]

[how-to]: http://www.cyberciti.biz/tips/howto-linux-unix-bash-shell-setup-prompt.html


# Example prompts

with colors:
```bash
export PS1="\[\033[36m\]\u:\[\033[1;33m\]\W\[\033[0;36m\]\$git_branch\[\033[m\]\$ "
```

plain:
```bash
export PS1="\u:\W\$git_branch\$ "
```


## Usage Tips

To view other user's tips, please check the
[Usage Tips](https://github.com/jimeh/git-aware-prompt/wiki/Usage-Tips) wiki
page. Or if you have tips of your own, feel free to add them :)


## Thanks

This is a modified version of a script by [Jim Myhrberg](https://github.com/jimeh/git-aware-prompt).


## License

[CC0 1.0 Universal](http://creativecommons.org/publicdomain/zero/1.0/)
