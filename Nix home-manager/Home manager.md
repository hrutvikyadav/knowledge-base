## First steps
### Installing as standalone config with separate `home.nix` file 
1. Add the `unstable` or `stable` *home-manager channel*
   ```bash
   nix-channel --add https://github.com/nix-community/home-manager/archive/master.tar.gz home-manager
   ```
2. Update channels
   ```bash
   nix-channel --update
   ```
3.  Install home-manager 
   ```bash
   nix-shell '<home-manager>' -A install
   ```
4. Add to `.zshrc` to indicate home-manager will not manage shell configuration with something like `programs.zsh.enable` in `home.nix`
### Installing as `NixOS` module inside `configuration.nix`

---
![[Pasted image 20230802151822.png]]