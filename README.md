# VSCODE-YAML-LSP

- 目標
独自のConfigを設定する際に候補が出るようになってほしい

- なぜほしいのか

1. 書き方を覚えるのがつらい
2. 自身を含めユーザーが簡単に使えるようにしたい


## どのように実現するか

- 作業環境は`VisualStudio Code`を前提とする
- Language Serverを利用する [YAML Language Server](https://github.com/redhat-developer/yaml-language-server)
- 補間の定義を`settings.json`を通して渡すことができるのでProjectに定義ファイルを含める


### VSCode settings

設定パラメータは[Settings](https://github.com/redhat-developer/yaml-language-server#language-server-settings)に書いてある。

ここでは以下のように使う
`yaml.schemas`はkeyにJSON Schemaのpath(local or remote)を指定しvalueには設定を有効にするファイル名のglobpatternを入力する。

```json
{
    "yaml.format.enable": true,
    "yaml.completion": true,
    "yaml.validate": true,
    "yaml.hover": true,
    "yaml.schemas": {
        "http://json.schemastore.org/swagger-2.0": ["*swagger.yaml", "*swagger.yml"],
    },
}
```


### YAML LSP ExtensionはJSON Schemaで補完する書式を決める

##### What is JSON Schema

[Advantage](https://json-schema.org/)

- Describes youe existing data format
- Provides clear hunam- and machine- readable documentation
- Validates data which is useful for
  * automated testing
  * Ensuring quality of client submitted data


[Reference](
https://json-schema.org/understanding-json-schema/reference/)

##### Starting the schema

- `$schema` keyword states 
- `$id` keyword defines a URI
- `title` and `description` annotation keywords are descriptive only
- `type` validation keyword defines the first constraint

#### Defining the properties

- `properties` validation keyword
  + `description`
  + `type`

##### Type string

- `minLength`
- `maxLength`
- `pattern`
- `format`

##### Type integer / number

- `exclusiveMinimum`
- `minimum`
- `maximum`

##### Type array

- `items`
  + `type`
- `minItems` number
- `uniqueItems` bool

##### TypeObject

- `type` = object
- `properties` recrusive


## Examples

##### DBConfig

This is configure of connect to DataBase with considing timezone location.

```go
type DBConfig struct {
	Name          string `yaml:"name"`
	Driver        string `yaml:"driver"`
	Hostname      string `yaml:"host"`
	Username      string `yaml:"user"`
	Password      string `yaml:"pass"`
	Database      string `yaml:"database"`
	Timezone      string `yaml:"tz"`
	MaxConnection int    `yaml:"max_conn"`
}
```


## VSCode Tips

- Format the JSON on VS Code 
  - focus the window and `RightClick -> FormatDocument(Shift+Alt+F)`
- Suggestion yaml
  - `Ctrl+Space`



## References

- VSCode Extension[yaml-language-server](https://github.com/redhat-developer/yaml-language-server)
- [JSON Schema](https://json-schema.org/)
- [Understanding JSON Schema](https://json-schema.org/understanding-json-schema/index.html#)
- [Standard Go Project Layout](https://github.com/golang-standards/project-layout)
