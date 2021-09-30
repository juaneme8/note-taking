# Settings
> Basado en https://code.visualstudio.com/docs/getstarted/settings

## User Settings o Workspace Settings
En Visual Studio Code contamos tanto con **User Settings** (aplican globalmente en todas las instancias que abramos) como **Workspace Settings** (aplican sólo para el proyecto en el cual las establezcamos, siendo capaces de sobrescribir a las de usuario). 

## Acceso a Settings
Podemos acceder las Settings tanto con **Command Palette** (Ctrl+Shift+P) escribiendo **Preferences: Open Settings** o bien usando el atajo (Ctrl+,). Veremos luego una selectora entre User y Workspace.

El archivo `settings.json` correspondiente a las User Settings se encuentra en `%APPDATA%\Code\User\settings.json`

## UI vs JSON
Si hacemos click en una hoja de papel ubicada en el margen superior derecho podremos pasar togglear entre la UI y el JSON.
Si queremos trabajar siempre con el JSON podemos cambiarlo con lo siguiente:
```json
// Determines which settings editor to use by default.
  //  - ui: Use the settings UI editor.
  //  - json: Use the JSON file editor.
  "workbench.settings.editor": "json"
```
En la UI los cambios respecto a las default settings los veremos con una línea azul.

# Reseteo de Settings
 Si por algún motivo deseamos borrar todas las User Settings que hayamos configurado bastará borrar todo entre las dos llaves en el `settings.json` 

# Mis Settings Globales
```json
{
	//Para evitar que nos ponga espacios extra al copiar texto y pegarlo en otro lado (por ejemplo en StackEdit)
    "editor.copyWithSyntaxHighlighting": false,
    //Configuración de fuente
    "editor.fontFamily": "'Fira Code','Cascadia Code PL','Victor Mono',Consolas, 'Courier New', monospace",
    "editor.fontLigatures": true,
    //Para evitar que muestre el minimap al costado
    "editor.minimap.enabled": false,
    //Para evitar confirmación al eliminar
    "explorer.confirmDelete": false,
    //Esta configuración se agrega automáticamente si usamos tabnine
    "tabnine.experimentalAutoImports": true,    
    //Configuración de tema
    "workbench.colorTheme": "City Lights",
    //Para evitar que si no editamos un archivo y presionamos en otro este se cierre y ocupe el nuevo esa pestaña
     "workbench.editor.enablePreview": false,
    //Para que si presionamos CTRL+. nos abra un JSON y no UI
    "workbench.settings.editor": "json",

    //Para trabajar con PowerShell como terminal integrada y no ver mensaje de bienvenida
    "terminal.integrated.shell.windows": "C:\\Program Files\\PowerShell\\7\\pwsh.exe",
	"terminal.integrated.shellArgs.windows": ["-NoLogo"],

}
//Para evitar mensaje en PC del trabajo: 
// Problems loading reference 'http://json.schemastore.org/eslintrc': Unable to load schema from 'http://json.schemastore.org/eslintrc': Request vscode/content failed unexpectedly without providing any details.
"http.proxy": "http://172.30.221.240:8080",
"http.proxyAuthorization": null,
"http.proxyStrictSSL": true,
```

# Config Goncy
"editor.fontFamily": "JetBrains Mono, Dank Mono, Fira Code, Operator Mono Lig, Hack"