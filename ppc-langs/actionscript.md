# ActionScript

Язык, который используется в плагинах в решениях Adobe. Раньше еще на нем писали Adobe Flash Player приложения (которые сейчас не запустишь в браузере).

Несмотря на узкое применение этого языка, и на нем порой что-то приходится писать (

## Setup IDE

Писать код и запускать можно в VSCode.

Для этого нам понадобиться ActionScript SDK и расширение для поддержки ActionScript в VSCode. Сейчас за всю эту поддержку отвечает [BowlerHatLLC](https://github.com/BowlerHatLLC/vscode-as3mxml).

Как установить ActionScript SDK (я выбрал Feathers SDK) — [инструкция](https://github.com/BowlerHatLLC/vscode-as3mxml/wiki/Choose-an-ActionScript-SDK-for-the-current-workspace-in-Visual-Studio-Code).

Другой вариант (более распространенный) — [Apache Flex](https://flex.apache.org/installer.html).

Выбираем [расширение](https://marketplace.visualstudio.com/items?itemName=bowlerhatllc.vscode-as3mxml) для VSCode — ActionScript & MXML.

## Create Project

```
$ mkdir src
$ touch src/Main.as src/Main-app.xml asconfig.json
```

Main.as:

```actionscript
package
{
    import flash.display.Sprite;

    public class Main extends Sprite {

        public function Main() {
            super();

            trace("Hello, World");
        }

    }
}
```

Main-app.xml:

```markup
<?xml version="1.0" encoding="utf-8" ?>
<application xmlns="http://ns.adobe.com/air/application/24.0">
	<id>com.example.Main</id>
	<versionNumber>0.0.0</versionNumber>
	<filename>Main</filename>
	<name>Main</name>
	<initialWindow>
		<content>[Path to content will be replaced by Visual Studio Code]</content>
		<visible>true</visible>
	</initialWindow>
</application>
```

asconfig.json:

```json
{
	"config": "air",
	"compilerOptions": {
		"source-path": [
			"src"
		],
		"output": "bin/Main.swf"
	},
	"mainClass": "Main",
	"application": "src/Main-app.xml"
}
```

Ну и жмем build project (cmd+shift+b -> compile debug). У нас в bin будет лежать swf файл.

И жмем Run. В консоли увидим Hello World.
