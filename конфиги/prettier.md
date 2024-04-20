>[!info]
>**Prettier — это средство для форматирования кода, которое нацелено на использование жёстко заданных правил по оформлению программ.**
Оно форматирует код автоматически, обеспечивая соблюдение предустановленных правил форматирования кода для проекта. Это позволяет сфокусироваться на более важных вещах при написании программного кода.

You can configure Prettier via (in order of precedence):
- A `"prettier"` key in your `package.json` file.
- A `.prettierrc` file written in JSON or YAML.
- A `.prettierrc.json`, `.prettierrc.yml`, `.prettierrc.yaml`, or `.prettierrc.json5` file.
- A `.prettierrc.js`, or `prettier.config.js` file that exports an object using `export default` or `module.exports` (depends on the [`type`](https://nodejs.org/api/packages.html#type) value in your `package.json`).
- A `.prettierrc.mjs`, or `prettier.config.mjs` file that exports an object using `export default`.
- A `.prettierrc.cjs`, or `prettier.config.cjs` file that exports an object using `module.exports`.
- A `.prettierrc.toml` file.

## Переписать правила для определенного файла или формата

```json
{ 
"semi": false,
 "overrides": [
	  { 
	  "files": "*.test.js", "options": { "semi": true } 
	  }, 
	  { 
	  "files": ["*.html", "legacy/**/*.js"], "options": { "tabWidth": 4 }
	   } 
	] 
}
```


##  Basic Configuration

```JSON
{ 
"trailingComma": "none", // ставить ли запятую после последнего елементов в массиве и объектах
 "tabWidth": 4, //Определяет ширину табуляции (в пробелах)
 "semi": true, //Определяет, должны ли точки с запятой быть добавлены в конец выражений 
 "singleQuote": true, //Определяет, должны ли использоваться одинарные кавычки вместо двойных кавычек для строковых литералов
 "printWidth": 80, // максимальная ширина кода, после которой он будет перенесен на другую строку
 "bracketSpacing": true, //Пробелы до и после своиств в объектах { something }.
 "arrowParens": true // - "always"` - Always include parens. Example: `(x) => x`"avoid"  
 //Omit parens when possible. Example: `x => x`
}
```