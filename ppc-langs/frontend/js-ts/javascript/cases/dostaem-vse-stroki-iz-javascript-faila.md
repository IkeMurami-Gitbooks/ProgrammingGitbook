# Достаем все строки из javascript-файла

package.json:

```json
{
  "name": "extract-strings",
  "version": "1.0.0",
  "description": "",
  "license": "ISC",
  "author": "",
  "type": "commonjs",
  "main": "extract-strings.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "@babel/parser": "^7.28.5",
    "@babel/traverse": "^7.28.5"
  }
}

```

extract-strings.js:

```javascript
#!/usr/bin/env node
/**
 * extract-strings.js
 * ------------------
 * Скрипт читает один (или несколько) .js/.jsx/.ts/.tsx файлов,
 * разбирает их в AST и выводит все найденные литералы‑строки.
 *
 * Поддерживает:
 *   • обычные строки:  "text", 'text'
 *   • шаблоны без интерполяций: `pure template`
 *   • шаблоны с интерполяциями: `${expr}` – выводит только «сырые» части,
 *                               а также сохраняет выражения, если нужно.
 *
 * Запуск:
 *   node extract-strings.js path/to/file.js […другие файлы]
 */

const fs = require('fs');
const path = require('path');
const parser = require('@babel/parser');
const traverse = require('@babel/traverse').default;

// ------------------------------------------------------------------
// 1. Чтение и парсинг файла
// ------------------------------------------------------------------
function parseFile(filePath) {
  const code = fs.readFileSync(filePath, 'utf8');

  // Опции парсера: поддерживаем почти любой современный синтаксис.
  const ast = parser.parse(code, {
    sourceType: 'unambiguous', // auto-detect module vs script
    plugins: [
      'jsx',
      'typescript',
      'classProperties',
      'objectRestSpread',
      'optionalChaining',
      'nullishCoalescingOperator',
      'decorators-legacy',
      'dynamicImport',
      'numericSeparator',
      // добавить, если нужен, например, "exportDefaultFrom"
    ],
  });

  return ast;
}

// ------------------------------------------------------------------
// 2. Обход AST и сбор строк
// ------------------------------------------------------------------
function collectStrings(ast) {
  const strings = [];

  traverse(ast, {
    // Обычные строковые литералы: "abc", 'abc'
    StringLiteral({ node }) {
      strings.push(node.value);
    },

    // Шаблонные литералы: `abc`, `a${b}c`
    TemplateLiteral({ node }) {
      // Мы хотим собрать «сырые» части (quasis)
      // и, при желании, отдельные выражения.
      const rawParts = node.quasis.map(q => q.value.cooked);
      const exprCount = node.expressions.length;

      if (exprCount === 0) {
        // Чистый шаблон без интерполяций – просто строка
        strings.push(rawParts.join(''));
      } else {
        // Содержит выражения. Можно решить, как их представлять.
        // Ниже – вариант, где мы сохраняем шаблон с placeholder'ами.
        const placeholder = '${...}';
        const combined = rawParts.reduce((acc, cur, i) => {
          acc += cur;
          if (i < exprCount) acc += placeholder;
          return acc;
        }, '');
        strings.push(combined);
      }
    },

    // Если нужно собирать строки из импортов вроде:
    // import msg from "./locales/ru.json";
    // То можно добавить обработку ImportDeclaration, но обычно это не требуется.
  });

  return strings;
}

// ------------------------------------------------------------------
// 3. Основная часть: обработка аргументов командной строки
// ------------------------------------------------------------------
function main(output_file_name) {
  const args = process.argv.slice(2);
  if (args.length === 0) {
    console.error('Usage: node extract-strings.js <file1.js> [file2.js …]');
    process.exit(1);
  }

  const allStrings = [];

  for (const arg of args) {
    const absolute = path.resolve(arg);
    if (!fs.existsSync(absolute)) {
      console.warn(`File not found: ${absolute}`);
      continue;
    }

    try {
      const ast = parseFile(absolute);
      const strings = collectStrings(ast);
      allStrings.push(...strings);
    } catch (e) {
      console.error(`Error parsing ${absolute}:`, e.message);
    }
  }

  // Убираем дубликаты (по желанию)
  const uniq = Array.from(new Set(allStrings));

  // Выводим каждый элемент в отдельной строке (удобно для дальнейших пайпов)
  // uniq.forEach(str => console.log(str));

    const map = uniq.reduce((acc, str, idx) => {
        // Генерируем простой ключ: msg_001, msg_002 …
        const key = `msg_${String(idx + 1).padStart(3, '0')}`;
        acc[key] = str;
        return acc;
    }, {});

    // Записываем JSON
    fs.writeFileSync(output_file_name, JSON.stringify(map, null, 2), 'utf8');
}

// Usage: node extract-strings.js path/to/file.js […другие файлы]
// Usage: node extract-strings.js src/**/*.{js,jsx,ts,tsx}
main('output.json');

```
