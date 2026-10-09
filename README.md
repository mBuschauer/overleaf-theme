# Overleaf Theme
This is just a simple Overleaf LaTex editor inspired light theme. It's based on the updated OverLeaf UI. I was about to start working on my QM Homework when I thought of doing this instead (talk about procrastination!). Anyway, the main reason for creating this was to just make the editor look like Overleaf, because I don't like writing my articles in an online based editor if it's just solo work. I prefer to do it offline where I can have all the compile time in the world and not worry about running out of it.

**Anyway. I hope you enjoy it!**

## Tasks
- [ ] Add screenshots of the theme (once I finished aforementioned QM homework)
- [x] Fix the styling of non .tex files and non-pdf panels

PS: Feel free to fork and create a dark mode if you want to.

# Installation
I only included an installation for NixOS (because that's what I use). If you ever want to send me installation instructions for any other system, feel free.
## Nix Installation
Add it as an input to flake.nix
```nix
inputs.overleaf-theme = {
    url = "github:mbuschauer/overleaf-theme";
    flake = false;
};
```
Then in home manager, define the plugin, install and activate it (I included settings from my personal preference).
```nix
{
  config,
  lib,
  pkgs,
  inputs,
  ...
}:
let
  overleaf-theme = pkgs.vscode-utils.buildVscodeExtension {
    pname = "overleaf-theme";
    version = "0.0.4";
    src = inputs.overleaf-theme;
    sourceRoot = "source";
    vscodeExtPublisher = "marco";
    vscodeExtName = "overleaf-theme";
    vscodeExtUniqueId = "marco.overleaf-theme";
  };
in
{
  programs.vscode = {
    enable = true;
    package = pkgs.vscode;
    profiles.default = {
      extensions =
        with pkgs.vscode-extensions;
        [
          overleaf-theme
        ];
      userSettings = {
        "workbench.experimental.modernUI" = false;
        "[latex]"."editor.wordWrap" = "on";
        "latex-workshop.latex.autoClean.run" = "onBuilt";
        "latex-workshop.latex.clean.method" = "glob";
        "latex-workshop.latex.clean.fileTypes" = [
          "*.aux"
          "*.bbl"
          "*.blg"
          "*.idx"
          "*.ind"
          "*.lof"
          "*.lot"
          "*.out"
          "*.toc"
          "*.fls"
          "*.log"
          "*.fdb_latexmk"
          "*.nav"
          "*.snm"
          "*.vrb"
          "*.synctex(busy)"
          "*.synctex.gz(busy)"
        ];
        "latex-workshop.view.pdf.viewer" = "tab";
        "latex-workshop.view.pdf.color.light.backgroundColor" = "#495365";
        "latex-workshop.formatting.latex" = "tex-fmt";

        "workbench.colorTheme" = "mbuschauer.overleaf";
        "workbench.preferredLightColorTheme" = "mbuschauer.overleaf";
        "workbench.preferredDarkColorTheme" = "mbuschauer.overleaf";

        "editor.fontFamily" = "'DejaVu Sans Mono', monospace";
        "editor.fontSize" = 13;
        "editor.lineHeight" = 19;
        "editor.minimap.enabled" = false;
        "editor.showFoldingControls" = "always";

        # Terminal cursor
        "terminal.integrated.cursorStyle" = "block";
        "terminal.integrated.cursorBlinking" = true;
      };
      keybindings = [
        {
          key = "ctrl+b";
          command = "editor.action.insertSnippet";
          args = {
            snippet = "\\\\textbf{\${TM_SELECTED_TEXT:$0}}";
          };
          when = "editorTextFocus && !editorReadonly && editorLangId == 'latex'";
        }
        {
          key = "ctrl+i";
          command = "editor.action.insertSnippet";
          args = {
            snippet = "\\\\textit{\${TM_SELECTED_TEXT:$0}}";
          };
          when = "editorTextFocus && !editorReadonly && editorLangId == 'latex'";
        }
        {
          key = "ctrl+u";
          command = "editor.action.insertSnippet";
          args = {
            snippet = "\\\\underline{\${TM_SELECTED_TEXT:$0}}";
          };
          when = "editorTextFocus && !editorReadonly && editorLangId == 'latex'";
        }
      ];
    };
  };
}
```