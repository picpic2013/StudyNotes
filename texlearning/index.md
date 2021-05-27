# 学习tex的笔记

## VS Code 的环境搭建

- 编辑`C:\Users\{用户名}\AppData\Roaming\Code\User\settings.json`文件

- 在 `{}` 中加入以下内容

    ~~~json
    "latex-workshop.view.pdf.viewer": "tab",
    "[latex]": {

        "editor.formatOnPaste": false,
        "editor.suggestSelection": "recentlyUsedByPrefix", 
        "editor.wordWrap": "on"
    }, 
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "xelatexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "-xelatex",
                "%DOCFILE%"
            ],
            "env": {}
        },
        {
            "name": "latexmk_rconly",
            "command": "latexmk",
            "args": [
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ],
            "env": {}
        },
        {
            "name": "rnw2pdf",
            "command": "Rscript",
            "args": [
                "-e",
                "knitr::knit2pdf('%DOCFILE%')"
            ],
            "env": {}
        }
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "latexmk 🔃",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "latexmk (latexmkrc)",
            "tools": [
                "latexmk_rconly"
            ]
        },
        {
            "name": "latexmk (xelatex)",
            "tools": [
                "xelatexmk"
            ]
        },
        {
            "name": "Compile Rnw files",
            "tools": [
                "rnw2pdf"
            ]
        }
    ],
    "latex-workshop.latex.recipe.default": "lastUsed",
    "latex-workshop.intellisense.unimathsymbols.enabled": true,
    "latex-workshop.intellisense.package.enabled": true
    ~~~