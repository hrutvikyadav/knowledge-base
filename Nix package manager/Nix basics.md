## Architecture
[[Nix structure.excalidraw]]

---
## Testing out packages in `nix-shell` before actually installing on system
`nix-shell` can be used to test out packages in an isolated shell before actually installing them.
1. Create a directory to set up a test environment
2. Create a `shell.nix` file [^alt]
3. run `nix-shell` from this directory [^alt2]
![[Pasted image 20230728153821.png]]

## Generalized `shell.nix`
```
{ pkgs ? import <nixpkgs> {} }:
pkgs.mkShell {
  buildInputs = [ pkgs.neofetch pkgs.tree pkgs.hello pkgs.tmux pkgs.fzf pkgs.ripgrep pkgs.fd pkgs.neovim pkgs.git pkgs.jq pkgs.curl pkgs.wget pkgs.zsh pkgs.asciinema pkgs.lolcat ];  # Add other packages as needed
}
```
> [!extras]
> `rustup`, `ocaml`, 

## Garbage collection
1. `nix-store --gc --print-dead` print unreferenced packages that will be removed
2. `nix-store --gc` actually remove them
3. `nix-env --list-generations` list all existing generations in profiles
4. `nix-collect-garbage -d` delete older generations

## References
[Search nix packages](https://search.nixos.org/packages)

---
[^alt]: the `shell.nix` can also live in users home folder so that running `nix-shell` anywhere will read this file
[^alt2]: instead of building from the file, it is possible to directly run nix-shell with `-p` option and package name to install