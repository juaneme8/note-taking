# Starship Prompt
> Basado en el [video](https://youtu.be/Tq-kmohWYnU) 

Desde Windows store instalar **Windows Terminal**

Instalación scoop: `iwr -useb get.scoop.sh | iex`

Ejecutar
1.  `scoop install starship`
    
## Instalar Nerd Font:
Descargamos de [NerdFonts](https://www.nerdfonts.com/font-downloads) la fuente **FireCode Nerd Font** que es la que nos sugiere la documentación de Starship 
Fira Code Bold Nerd Font Complete Mono.ttf


####   
PowerShell

Add the following to the end of  `Microsoft.PowerShell_profile.ps1`. You can check the location of this file by querying the  `$PROFILE`  variable in PowerShell. Typically the path is  `~\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`  or  `~/.config/powershell/Microsoft.PowerShell_profile.ps1`  on -Nix.

```
Invoke-Expression (&starship init powershell)
```


echo $profile
devuelve C:\Users\Juan Lauria\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

Test-Path $profile
obtenemos false

Como no existe creamos un nuevo perfil
New-Item -path $profile -type file –force

notepad $PROFILE
pegamos ```
Invoke-Expression (&starship init powershell)

```

Cerramos la terminal y la volvemos a abrir y ya veremos algunso cambios

https://starship.rs/config/#prompt


cd .\.config\
notepad starship.toml

vamos a git branch
copiamos 

```
[git_branch]
symbol = "🌱 "
truncation_length = 4
truncation_symbol = ""
```

y lo pegamos en el archivo

repetimos lo mismo para todo esto


[git_branch]
symbol = "🌱 "
truncation_length = 4
truncation_symbol = ""

[git_commit]
commit_hash_length = 4
tag_symbol = "🔖 "

[git_state]
format = '[\($state( $progress_current of $progress_total)\)]($style) '
cherry_pick = "[🍒 PICKING](bold red)"

[git_status]
conflicted = "🏳"
ahead = "🏎💨"
behind = "😰"
diverged = "😵"
untracked = "🤷‍"
stashed = "📦"
modified = "📝"
staged = '[++\($count\)](green)'
renamed = "👅"
deleted = "🗑"

[nodejs]
format = "via [🤖 $version](bold green) "


# Fuente
https://github.com/JetBrains/JetBrainsMono/releases/tag/v2.225
Instalar como zip y luego la v JetBrainsMono-Bold extrabold, regular, thin, light


# expand selection
eprmite selec

# matching pair control .
salta de uno a otro


ayu mirage border
change-case

Instalamos Powershell 7

en vscode agregamos al json

  "terminal.integrated.shell.windows": "C:\\Program Files\\PowerShell\\7\\pwsh.exe",
  "terminal.integrated.shellArgs.windows": ["-NoLogo"]