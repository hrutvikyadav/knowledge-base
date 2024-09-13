---
id: AutohotkeySetup
aliases:
  - AutohotkeySetup
tags: []
---

# AutohotkeySetup

- Install Autohotkey from [their site](https://www.autohotkey.com/download/ahk-v2.exe)
    make note of the **installation path**, in this case `C:\Users\Admin\AppData\Local\Programs\AutoHotkey\v2\AutoHotkey64.exe`
- Install autohotkey language server; setup neovim for development
    References - [Standalone Installation for language server](https://github.com/thqby/vscode-autohotkey2-lsp?tab=readme-ov-file#use-in-other-editors), [Configure nvim-lspconfig](https://github.com/thqby/vscode-autohotkey2-lsp?tab=readme-ov-file#nvim-lspconfig)
    ```sh
    ~/pers ❯ mkdir vscode-autohotkey2-lsp
    ~/pers ❯ cd vscode-autohotkey2-lsp
    ~/pers/vscode-autohotkey2-lsp ❯ curl -L -o install.js https://raw.githubusercontent.com/thqby/vscode-autohotkey2-lsp/main/tools/install.js
    
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 32519  100 32519    0     0  70044      0 --:--:-- --:--:-- --:--:-- 69933
    ~/pers/vscode-autohotkey2-lsp ❯ ls                                                                                                                                                                                                                       via  v21.5.0
    install.js
    ~/pers/vscode-autohotkey2-lsp ❯ node install.js                                                                                                                                                                                                          via  v21.5.0
    thqby.vscode-autohotkey2-lsp v2.4.9 has been installed.
    ~/pers/vscode-autohotkey2-lsp ❯ ls                                                                                                                                                                                                          is 📦 v2.4.9 via  v21.5.0
    CHANGELOG.md  LICENSE.txt  README.md  install.js  package.json  package.nls.json  package.nls.zh-cn.json  server  syntaxes
    ~/pers/vscode-autohotkey2-lsp ❯ tree . -I node_modules                                                                                                                                                                                      is 📦 v2.4.9 via  v21.5.0
    .
    ├── CHANGELOG.md
    ├── LICENSE.txt
    ├── README.md
    ├── install.js
    ├── package.json
    ├── package.nls.json
    ├── package.nls.zh-cn.json
    ├── server
    │   └── dist
    │       ├── ahkProvider.ahk
    │       └── server.js
    └── syntaxes
        ├── ahk2.d.ahk
        ├── ahk2.json
        ├── ahk2_common.json
        ├── ahk2_h.d.ahk
        ├── ahk2_h.json
        ├── winapi.d.ahk
        └── zh-cn
            ├── ahk2.d.ahk
            ├── ahk2.json
            ├── ahk2_h.d.ahk
            └── ahk2_h.json
    
    4 directories, 19 files
    ~/pers/vscode-autohotkey2-lsp ❯ ls server/dist/server.js                                                                                                                                                                                    is 📦 v2.4.9 via  v21.5.0
    server/dist/server.js
    ~/pers/vscode-autohotkey2-lsp ❯ cd server/dist                                                                                                                                                                                              is 📦 v2.4.9 via  v21.5.0
    vscode-autohotkey2-lsp/server/dist ❯ pwd                                                                                                                                                                                                                 via  v21.5.0
    /home/hrutvik_/pers/vscode-autohotkey2-lsp/server/dist
    vscode-autohotkey2-lsp/server/dist ❯
    ```
- Create and execute an `.ahk` script
    Sample script
    ```ahk
    ; ./script.ahk
    ; Easy to make GUIs

    MyGui := Gui()
    MyGui.Add("Text",, "Enter your name")
    MyGui.Add("Edit", "w150 vName")
    MyGui.Add("Button",, "OK").OnEvent("Click", SayHello)
    MyGui.Show()
    Return
    
    SayHello(*)
    {
        Saved := MyGui.Submit()
        MsgBox "Hello " Saved.Name
        ExitApp
    }
    ```
    Run with <installation_path> ./script.ahk
