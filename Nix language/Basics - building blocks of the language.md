## Nix expressions
Piece of `nix` code is called a *nix expression*
Contents of a `.nix` file are also expression
Evaluating expressions produce a *nix value* [[#Nix values]]

Expressions can be evaluated using 
- `nix repl` command, prepending `:p` to expression if output is not printed correctly
- using `nix-instantiate -eval file.nix` with optional `--strict` flag to *print evaluated output value* accurately in some cases
   > If no filename is given as arg, it will look for `default.nix` file to read and evaluate expressions from
## Nix values
Values can be *assigned to names* and can be either one of
1. primitive data types
2. lists - space separated list of primitives ?
3. [[#Attribute sets]]
4. functions
## Nix language constructs and features
### Attribute sets
These are collection of **name-value pairs** where *names are unique*
```nix
{
	string = "hello";
	integer = 1;
	float = 3.141;
	bool = true;
	null = null;
	list = [ 1 "two" false ];
	attribute-set = {
		a = "hello";
		b = 2;
		c = 2.718;
	    d = false;
	}; # comments are supported
}
```
#### Recursive attribute sets
Sets can be recursive - this allows to refer to attributes declared in a set while being inside that set
```nix
rec {
	one = 1;
	two = one + 1;
	three = two + 1;
}
```
Here attribute `two` and `three` refer to names assigned prior to them
Above code evaluates to `{ one = 1; three = 3; two = 2; }`
#### Attribute access and assignment with `.` operator
Set attributes as well as nested attributes can be accessed with `<set>.<attr>`
```nix
let
	attrset = { a = { b = { c = 1; }; }; };
in
	attrset.a.b.c
```
👆 evaluates to `1`
```nix
{ a.b.c = 1; }
```
👆 evaluates to `{ a = { b = { c = 1; }; }; }`
### Lexically scoped bindings with `let .. in ..`
Assign names in `let ..` expression for repeated use inside `in ..` expression
The names will go out of scope after the `in ..` block ends
```nix
let
	b = a + 1;
	a = 1;
in
	a + b
```
### Access set attributes with `with <set>; ...` instead of using `set.xyz`
```nix
# instead of
let
	a = { x=1; y=2; z=3; };
in
	[a.x a.y a.z]

# use `with; ...` to avoid repeatedly referencing the set name in the expression following the ;
let
	a = {x=1; y=2; z=3};
in
	with a; [x y z]
```
### Use values of names in existing scope assigned to the same name in a nested scope with `inherit ...`
Used for conveniently *assigning same values to same name* **in a nested scope**
```nix
let
	x = 1;
	y = 2;
in
{
	inherit x y;
}
```
👆 evaluates to `{ x=1; y=2; }` as `inherit x y;` is equivalent to `x = x; y = y;`
> Use `inherit (...) ...` to inherit names from specified attribute `set`
```nix
let
	a = { x = 1; y = 2; };
in
{
	inherit (a) x y;
}
```
👆 evaluates to `{ x=1; y=2; }` as `inherit (a) x y;` is equivalent to `x = a.x; y = a.y;`
Also works inside `let inherit .. in ..` expressions
### String interpolation
Used to insert value of an expression in character string
```nix
let
	name = "HRX"
in
	"hello ${name}"
```
> [!warning]
> Only works if the expression in `${}` evaluates to string or other value that can be represented as string
### File system paths
These can be 
- Absolute
  These always start with `/`
  ```nix
  /absolute/path/dir 
  ```
- Relative
  These contain a `/` but cannot start with `/`
  ```nix
  ./.
  ```
  👆 This resolves to current dir - the trailing `.` doesn't change directory
  ```nix
  ./relative/dir
  # same as -
  relative/dir
  ```
  👆Evaluates to `pwd/relative/dir`
  ```nix
  ../.
  ```
  👆 Evaluates to parent dir
  > [!info]
  > Paths can be used in interpolated expressions – an [impure operation](https://nix.dev/tutorials/first-steps/nix-language#impurities)
### Search path
Also called angle bracket syntax
These named paths depend on the value of `$NIX_PATH` environment variable and corresponds to a file system path
Ex- `<nixpkgs>` corresponds to some revision of nixpkgs repo in filesystem
similarly, `<nixpkgs/lib>` corresponds to lib subdirectory of that path
> [!warning]
> *These are generally avoided*
> As these values depend on env vars and fs paths, they can vary per user and hence considered **impure and not reproducible**

### Indented or multi-line strings
Denoted by
```nix
''
multi
line
string
''
# equal amounts of whitespace gets trimmed
''
  one
   two
    three
''
# evaluates to "one\n two\n  three"
```
### Functions
A function always takes **exactly one argument**. Argument and function body are separated by a colon `:` as in
`arg : arg + 1`
`args` are used to *assign names to values*.
> Notably, values are not known in advance
> the names are used as placeholders that are filled when *calling a function*

`args` can be one of the following -
- Single arg
  `x: x + 1`
- Multiple args with **partial application / currying / nesting**
  `x: y: x + y`
- Attribute set arg
  `{ a, b }: a + b`
  - Using default attribute values
    `{ a, b ? 0 }: a + b`
    > Calling these with additional attribute name-value pair will cause error
    > See below for solution

  - Using additional allowed attributes
    `{ a, b, ...}: a + b`
    Calling this with more than specified attributes will not cause error but in this case these attrs will simply be discarded as they are not used in body
- Named attribute set arg
  Prepend or append a name to the argument separated by `@`
  `args@{ a, b, ... }: a + b + args.c`
  `{ a, b, ... }@args: a + b + args.c`
  Both of the above snippets are valid

All functions are *nameless* and are called `lambda`
They can be assigned to names
```nix
let
  f = x: x + 1;
in f

# evaluating this expression will result in a <LAMBDA>
```
### Calling functions
This is also called as applying functions (to arg) or **functional application**
Functions are called by writing args after the function separated by whitespace
```nix
let
	f = x: x.a;
	v = { a = 1; };
in
f v
```
> [!tip]
> Since function and argument are separated by white space, sometimes parentheses `( )` are required to achieve the desired result.
> Same is true for lists as list elements are also whitespace separated

```nix
(x: x + 1) 1

# evaluates to 2
```
```nix
let
	f = x: x + 1;
	a = 1;
in [ (f a) ]

# evaluates to [ 2 ], i.e. f applied to a; whereas,

let
	f = x: x + 1;
	a = 1;
in [ f a ]

# evaluates to [ <LAMBDA> 1 ], list with elements f and a
```
## Function libraries
Useful libraries are available for use
Two widely used ones are
1. `builtins` [reference](https://nixos.org/manual/nix/stable/language/builtins.html)
  These are implemented in C++ as part of the `Nix` language *interpreter*
  Most of these functions are available for use only via `builtins`, an exception to this is `import` which is available at the top level.
  It is used to evaluate nix expressions in a file and return the resulting value
  - If the path given to `import` is a directory, the `default.nix` file inside the dir is looked up and nix expressions inside it are evaluated instead
  > If the result of importing a file is a function, it can be applied to arguments immediately
  > ```nix
  > import ./file.nix 1
  > # where file.nix contains `x: x + 1`
  > ```
2. `<nikpkgs>.lib` [reference](https://nixos.org/manual/nixpkgs/stable/#sec-functions-library)
  These are written in `nix`
  Usually the `<nixpkgs>` attribute is assigned to the name `pkgs` and the `lib` functions are called using `pkgs.lib.f`
  ```nix
  let
	  pkgs = import <nixpkgs> {};
  in
	  pkgs.lib.strings.toUpper "search paths considered harmful"
  ```
## Impurities
Expressions covered so far are **pure expressions**
One **impurity** in Nix can be introduced when using __file system paths__ as `build inputs`
>Build inputs are `files ` that `build tasks` refer to in order to describe __how to derive new files__. When run, a build task will only have _access to explicitly declared_ build inputs.

The only way to specify build inputs in the Nix language is explicitly with
- File system paths - [[#Paths]]
- Dedicated functions for specifying build inputs - [[#Fetchers]]
> [!info]
> Nix and the Nix language _refer to files_ by their __content `hash`__
> If _file contents are not known in advance_, it’s unavoidable to _read files during expression evaluation_

> [!warning]
> `search paths` and the constant `builtins.currentSystem` are other examples of __impure expressions__ and are discouraged from use so as to not _break reproducibility_
### Paths
[[#File system paths]] used in [[#String interpolation]] will copy the _file contents_ to `nix store` and the expression will evaluate to a string with the ==**Nix store path** of that file==
```sh
$ echo 789 > data
```

```nix
"${./data}"

# evaluates to "/nix/store/h1qj5h5n05b5dl5q4nldrqq8mdg7dhqk-data"
```
For directories the same thing happens: The entire directory (including nested files and directories) is copied to the Nix store, and the evaluated string becomes the Nix store path of the directory
### Fetchers
The Nix language provides built-in impure functions to fetch files over the network during evaluation
- [`builtins.fetchurl`](https://nixos.org/manual/nix/stable/language/builtins.html#builtins-fetchurl)  
- [`builtins.fetchTarball`](https://nixos.org/manual/nix/stable/language/builtins.html#builtins-fetchTarball)
- [`builtins.fetchGit`](https://nixos.org/manual/nix/stable/language/builtins.html#builtins-fetchGit)
- [`builtins.fetchClosure`](https://nixos.org/manual/nix/stable/language/builtins.html#builtins-fetchClosure)
These functions evaluate to a ==**file system path in Nix store**==
```nix
builtins.fetchurl "https://github.com/NixOS/nix/archive/7c3ab5751568a0bc63430b33a5169c5e4784a0ff.tar.gz"

# evaluates to "/nix/store/7dhgs330clj36384akg86140fqkgh8zf-7c3ab5751568a0bc63430b33a5169c5e4784a0ff.tar.gz"
```
Some of these helpers perform extra operations for convenience, like automatically unpacking archives -
```nix
builtins.fetchTarball "https://github.com/NixOS/nix/archive/7c3ab5751568a0bc63430b33a5169c5e4784a0ff.tar.gz"

# evals to "/nix/store/d59llm96vgis5fy231x6m7nrijs0ww36-source"
```
## Derivations
A build task in Nix is called a _derivation_.
- The Nix language is used to describe build tasks.
- Nix runs build tasks to produce _build results_.
- Build results can in turn be used as inputs for other build tasks.

Nix language primitive to declare a build task is `derivation` - a built-in impure function
- This function is wrapped in the Nixpkgs build mechanism `stdenv.mkDerivation` which abstracts the complexity of build procedures
> The evaluation result of `derivation` (and `mkDerivation`) is an [[#Attribute sets]] with a certain structure and a special property: It can be used in [[#String interpolation]], and in that case evaluates to the Nix store path of its build result. See example below

```nix
let
	pkgs = import <nixpkgs> {};
in "${pkgs.nix}"

# evals to "/nix/store/sv2srrjddrp2isghmrla8s6lazbzmikd-nix-2.11.0"
```
> [!tip]
> String interpolation on derivations is used to refer to other build results as file system paths when declaring new build tasks.
> This allows constructing arbitrarily complex compositions of derivations with the Nix language.

