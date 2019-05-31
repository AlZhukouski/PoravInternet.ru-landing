# Gulp-Sass-BEM Starter Template

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)


Стартовый шаблон на сборщике пакетов gulp, использующий препроцессинг Sass и четко ориентированный на
разработку сайтов и верстку в методологии БЭМ. Шаблон вдохновлен аналогичным инструментом [Николая Громова](https://github.com/nicothin),
полностью взятым за основу, но сильно адаптированным для своих нужд.



` npm i ` - Установить зависимости.

` npm start ` - Запустить сборку, сервер и слежение за файлами.

` npm run build ` - Сборка проекта (сжатый вид, как результат работы).

` npm run blockcss ` - Сборка css-файлов для блоков отдельно в папку build/block-css.


Предполагается, что все команды вы выполняете в bash (для OSX и Linux это самый обычный встроенный терминал, для Windows это, к примеру, Git Bash). В Windows установку пакетов (`npm i`) нужно выполять в терминале, запущенном от имени администратора.



## Парадигма

- Используется именование классов, файлов и переменных по БЭМ.
- Каждый БЭМ-блок в своей папке внутри `./blocks/` (стили, js, картинки, разметка; обязателен только стилевой файл).
- Есть глобальные файлы: стилевые, js, шрифты, картинки внутри `./assets/`.
- Диспетчер подключения стилей `./assets/styles/style.scss`, туда необходимо импортировать глобальные стилевые файлы и стилевые файлы из папок блоков.
- Для разметки можно использовать микрошаблонизацию:
  `Разметка блоков может импортироваться в html страниц (./pages/*.html) с помощью синтаксиса @include "block/block.html"`
  А можно и не использовать.



## Блоки

Каждый блок лежит в `./blocks/` в своей папке. Каждый блок — как минимум, папка и одноимённый scss-файл.
При компиляции сборки стили из scss-файлов блока, скрипты из js-файлов блока и картинки блока из папки `img` автоматически включаются в сборку.
Обратите внимание, что перед использованием блока необходимо импортировать стилевой файл блока в `assets/styles/style.scss`

Возможное содержимое блока `example`:

```bash
example/               # Папка блока
  img/                 # Изображения, используемые блоком и обрабатываемые автоматикой сборки
  some-folder/         # Какая-то сторонняя папка, не обрабатываемая автоматикой
  example.scss         # Стилевой файл блока
  example--mod.scss    # Отдельный стилевой файл БЭМ-модификатора блока
  example.js           # js-файл блока
  example--mod.js      # js-файл для отдельного БЭМ-модификатора блока
  example.html         # Варианты разметки (как документация блока или как вставляемый микрошаблонизатором фрагмент)
  readme.md            # Какое-то пояснение
```



## Удобное создание нового блока

Предусмотрена команда для быстрого создания файловой структуры нового блока.

```bash
# формат: node createBlock.js ИМЯБЛОКА [доп. расширения через пробел]
node createBlock.js block-1 # создаст папку блока, block-1.html, block-1.scss и подпапку img/ для этого блока
node createBlock.js block-2 js pug # создаст папку блока, block-2.html, block-2.scss, block-2.js, block-2.pug и подпапку img/ для этого блока
```

Если блок уже существует, файлы не будут затёрты, но создадутся те файлы, которые ещё не существуют.



## Назначение папок

```bash
build/                     # Папка сборки, отсюда сервер транслирует файлы.
blocks/                    # Библиотека блоков проекта
assets/                    # Ресурсы проекта
  js/                      # - директория библиотек и скриптов, использующихся в проекте
  css/                     # - директория для дополнительных css-файлов, подключаемых отдельно от style.min.css (например, стили для плагинов)
  fonts/                   # - директория шрифтов проекта (будут автоматически скопированы в папку сборки)
  img/                     # - добавочные картинки проекта, не относящиеся к блокам
  styles/                  # - глобальные стилевые файлы 
     libs/                 # - добавочные scss плагинов и фреймворков
     fonts.scss            # - подключение и задание правил шрифтов
     mixins.scss           # - перечень примесей
     reset.scss            # - сброс стилей по умолчанию
     scaffolding.scss      # - общие стилевые правила для проекта
     style.scss            # - главный стилевой файл для импорта всех остальных
     svg_sprite.scss       # - описание классов для использования svg из спрайта в качестве background-image
     variables.scss        # - описание используемых переменных
pages/                     # - html-страницы проекта
```



## Комментирование для разработчиков

Для html-файлов можно использовать комментарии вида `<!--DEV Комментарий -->` — такие комментарии не попадут в собранный html.


## Удобный миксин для написания медиавыражений

Для scss-файлов существует миксин media(). Подробная инструкция по его использованию [здесь](https://habr.com/post/352686/).
Если вкратце, в ходе разработки вот этот код 

```bash
.class{
    @include media((
        height: (lg: 800px, md: 600px, sm: 300px)
    ));
}
```

превратится в этот:

```bash
@media only screen and (max-width: 1024px) {
    .class{
        height: 800px;
   }
}
@media only screen and (max-width: 768px) {
    .class{
        height: 600px;
   }
}
@media only screen and (max-width: 640px) {
    .class{
        height: 300px;
   }
}
```


## Поиск и исправление ошибок с eclint

Чтобы проверить код на соответствие форматирования из файла `.editorconfig`, установите eclint глобально:

```bash
npm install -g eclint
```

Использование:

```bash
eclint check "./blocks/**"        # - проверит, есть ли ошибки форматирования в указанной папке
eclint fix "./blocks/**"          # - исправит ошибки форматирования в указанной папке
```



## Встраивание критического CSS для index.html (тестирование)

В ходе выполнения команды ` npm run build ` в head файла index.html инлайново встраиваются стили, необходимые для отрисовки первого экрана, при этом основной style.min.css загружается асинхронно, не задерживая рендеринг контента. Используется модуль [Critical](https://github.com/addyosmani/critical) от [Эдди Османи](https://github.com/addyosmani).



## Использование svg-спрайта (тестирование)

Из используемых в проекте svg-изображений собирается два svg-спрайта: вида symbol и вида view. 
Первый собирается в файл `/build/svg/svgsprite.svg`. Содержимое файла svg инлайново вставляется в верстку с display:none и далее необходимые иконки как символы используются с помощью синтаксиса 

```bash
<svg class="inline-svg-icon">
    <use xlink:href="#cloud-computing"></use>
</svg>
```
 
Метод позволяет наиболее гибко управлять внешним видом иконки и ее трансформацией из css.
 
Метод view подходит, чтобы задавать svg-иконки из спрайта в качестве background-image. При компиляции создается файл `assets/styles/svg_sprite.scss`, в котором описываются классы для стилизуемых элементов. В этом файле не следует прописывать стили, так как он перезапишется при первой компиляции. Стоит отметить, что такой метод больше подходит, если все иконки изначально имеют одикаковый размер. 

[Подробнее о способах подключения svg-спрайтов](https://uwebdesign.ru/svg-sprites/).

[Подробнее о способе symbols и use xlink](http://dreamhelg.ru/2017/02/symbol-svg-sprite-detail-guide/)