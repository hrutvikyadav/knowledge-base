![[Pasted image 20230926151039.png]]

# Base tools
```sh
opam switch create latest 5.1.0
eval $(opam env --switch=latest)
```

# Platform tools
```sh
opam install dune merlin ocaml-lsp-server odoc ocamlformat utop dune-release
```

# Confirm
```sh
which ocaml
ocaml -version
```

![[Pasted image 20230926160920.png]]

![[Pasted image 20230926161226.png]]

![[Pasted image 20230926161414.png]]

## Finally
![[Pasted image 20230926165020.png]]

---