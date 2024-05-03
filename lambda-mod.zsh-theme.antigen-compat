#!/usr/bin/env zsh

local LAMBDA="%(?,%{$fg_bold[green]%}→,%{$fg_bold[red]%}→)"
if [[ "$USER" == "root" ]]; then USERCOLOR="red"; else USERCOLOR="yellow"; fi

# Git sometimes goes into a detached head state. git_prompt_info doesn't
# return anything in this case. So wrap it in another function and check
# for an empty string.
function check_git_prompt_info() {
    if type git &>/dev/null && git rev-parse --git-dir > /dev/null 2>&1; then
      if [[ -z $(git_super_status 2> /dev/null) ]]; then
            echo puto
            echo "%{$fg[blue]%}detached-head%{$reset_color%}
%{$fg[yellow]%}λ "
        else
            echo "$(git_super_status 2> /dev/null)
%{$fg_bold[cyan]%}λ "
        fi
    else
        echo "%{$fg_bold[cyan]%}λ "
    fi
}

function count_jobs() {
  jobs | awk '
    BEGIN { a = 0 }
    /^\[/ { a++ }
    END   { print a }
  '
}

function calc_disk_space() {
  df | awk '
    $6 == "/" {
      gsub("%", "", $5);
      print $5;
    }
  '
}


function juvocontext() {
  echo "󱙺 $(juvo config info --short)"
}

function kubecontext() {
  ctx=$(kubectl ctx -c)
  ns=$(kubectl ns -c)
  out=$ctx/$ns

  case "$out" in
    "juvo-ilab-prod/staging"|"juvo-ilab-prod/prod"|"juvo-dev/dev"|"juvo-dev/platform")
      out=$ns ;;
  esac
  echo "󱃾 $out"
}

function get_right_prompt() {
  local jobs_nbr=`count_jobs`
  local disk_perc=`calc_disk_space`

  if [ $jobs_nbr -gt 0 ]; then
    if [ $jobs_nbr -eq 1 ]; then
      echo -n "%{$fg_bold[yellow]%}⚙%{$reset_color%}"
    else
      echo -n "%{$fg_bold[yellow]%}⚙ $jobs_nbr%{$reset_color%}"
    fi
  fi

  if [ $disk_perc -gt 75 ]; then
    echo -n "%{$fg_bold[red]%} $disk_perc%%"
  fi

  echo -n "%{$reset_color%}"
}


PROMPT=$'\n'$LAMBDA'\
 %{$fg_bold[$USERCOLOR]%} %n\
 %{$fg_no_bold[red]%}$(juvocontext)\
 %{$fg_bold[green]%}$(kubecontext)\
 %{$fg_no_bold[magenta]%} %3~\
 $(check_git_prompt_info)\
%{$reset_color%}'

RPROMPT='$(get_right_prompt)'

# Format for git_prompt_info()
ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg[blue]%}⭠ "
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%}"
ZSH_THEME_GIT_PROMPT_DIRTY="⍟"
ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg_bold[green]%} ✔"

ZSH_THEME_GIT_PROMPT_STAGED="%{$fg[red]%}%{●$reset_color%}"
ZSH_THEME_GIT_PROMPT_CONFLICTS="%{$fg[red]%}%{✖$reset_color%}"
ZSH_THEME_GIT_PROMPT_CHANGED="%{$fg[blue]%}%{✚$reset_color%}"
ZSH_THEME_GIT_PROMPT_BEHIND="%{↓$reset_color%}"
ZSH_THEME_GIT_PROMPT_AHEAD="%{↑$reset_color%}"
ZSH_THEME_GIT_PROMPT_UNTRACKED="%{…$reset_color%}"

# Format for git_prompt_status()
ZSH_THEME_GIT_PROMPT_ADDED="%{$fg_bold[green]%}+"
ZSH_THEME_GIT_PROMPT_MODIFIED="%{$fg_bold[blue]%}!"
ZSH_THEME_GIT_PROMPT_DELETED="%{$fg_bold[red]%}-"
ZSH_THEME_GIT_PROMPT_RENAMED="%{$fg_bold[magenta]%}>"
ZSH_THEME_GIT_PROMPT_UNTRACKED="%{$fg_bold[cyan]%}?"

# Format for git_prompt_long_sha() and git_prompt_short_sha()
ZSH_THEME_GIT_PROMPT_SHA_BEFORE=" %{$fg_bold[white]%}[%{$fg_bold[blue]%}"
ZSH_THEME_GIT_PROMPT_SHA_AFTER="%{$fg_bold[white]%}]"
