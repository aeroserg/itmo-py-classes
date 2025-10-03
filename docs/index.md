# Добро пожаловать!

___Всем привет___: 📅 03/10/2025, 🕔 ~18:45

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Этапы сборки

    name: Сборка и публикация MkDocs сайта

        on:
        push:
            # branches: [ main ]

        jobs:
        build-and-deploy:
            name: Сборка и публикация
            runs-on: ubuntu-latest

            steps:
            - name: Получить код из репозитория
                uses: actions/checkout@v4

            - name: Установить Python
                uses: actions/setup-python@v4
                with:
                python-version: 3.12

            - name: Установить MkDocs
                run: pip install mkdocs

            - name: Установить Node.js
                uses: actions/setup-node@v4
                with:
                node-version: '18'

            - name: Установить и собрать CSS
                run: |
                npm ci
                npm run build-css

            - name: Собрать сайт
                run: mkdocs build

            - name: Опубликовать на GitHub Pages
                uses: peaceiris/actions-gh-pages@v4
                with:
                github_token: ${{ secrets.GITHUB_TOKEN }}
                publish_dir: ./site
                force_orphan: true 
