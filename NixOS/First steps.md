## Edit `/etc/nixos/configuration.nix` file to setup the system
```diff
+ # Install needed packages
+ environment.systemPackages = with pkgs; [
+ wget curl neofetch tree git zsh tmux fzf ripgrep fd neovim jq asciinema lolcat kitty oh-my-zsh
+ ];
```

## After modifying config file
To build the new config, make it the default config while booting, and make the changes take effect in the current session, run
```bash
nixos-rebuild switch
```
> [!warning]
> When *symlinking the store to profiles*, permission could be possibly denied - run with `sudo`

## Setup desktop environment - window manager

## Setup Terminal
### `Kitty`

Check system installed fonts with `kitty +list-fonts`

Add config file at `~/.config/kitty/kitty.conf`
```diff
+ font_family Hack Regular
+ bold_font   Hack Bold
+ italic_font Hack Italic

+ background_opacity 0.5
```
- Run `kitty +kitten themes`

### Setup zsh
1. Add `programs.zsh.enable = true;` in to enable zsh system-wide
2. Add `users.users.yourname.shell = pkgs.zsh;` to enable for user
3. To make ==zsh a valid shell==, add `environment.shells = with pkgs; [ zsh ];` This will add zsh to `/etc/shells`[^add-to-/etc/shells] which is readonly file.
4. Run `chsh -s $(which zsh)`

### Add `oh-my-zsh`
using nixpackages / curl script on their github
![[Pasted image 20230731171928.png]]

---
[^add-to-/etc/shells]: this is needed for `chsh -s (which zsh)` to work