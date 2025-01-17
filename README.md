# Отчет о выполнении дз / Инструкции к проверке

## Формат сообщений коммитов

-   Все коммиты должны соответствовать формату `conventional commits`. Проверка реализована в два этапа:
    -   Сообщения проверяются локально на прекоммит хуках[^1]
    -   Так же их проверяет флоу `commitLint`[^2]

## Автоматизация пулл реквестов

-   На каждый созданный (вручную) ПР настроен флоу `PR`, который делает автоматические проверки:[^3]
    -   Unit тесты
    -   e2e тесты
    -   Линтер
    -   Правильный формат коммит сообщений
-   Настроено ограничение на мерж в случае, если флоу `PR` не был успешно завершен[^4]
-   Так же флоу отрабатывает и на каждый новый коммит в ПР[^5]
-   Все результаты проверок видны на странице _ПР_[^6]

## Автоматизация релизного процесса

-   Флоу `Realease` запускается автоматически при появлении нового тэга релизного формата `v<число>`[^7]
-   Автоматически создается новое issue с пометкой `RELEASE`, которое обладает всеми необходимы полями с информацией:[^8]
    -   Версия релиза, которая соответствует релизному тэгу
    -   Указывается автор и дата релиза
    -   changelog по истории коммитов от предыдущего релизного тэга
-   Запускаются тесты, результаты которых можно увидеть в комментариях в конкретном issue[^9]
-   Если все проверки прошли успешно, то в ветке `gh-pages` создается новый билд и происходит деплой с помощью `gh-pages`[^10]
-   Результат успешного деплоя так же записывается в комментарий в issue, а затем происходит автозакрытие issue[^11]
-   Если флоу запуститься с тем же тэгом, то он отработает корректно, не создавая новое issue, а дополняя уже имеющийся[^12]

## Автоматизация реализована с помощью GitHub Actions

-   Секреты (кроме дефолтного) не используются, все защищено
    [^1]: Проверятся локально. Попробуйте сделать коммит с сообщением в неправильном формате
    [^2]: Проверяется визуально - в разделе _Actions_ должен отрабатывать флоу `commitLint`
    [^3]: Создайте ПР из `dev` ветки c искусственно созданной ошибкой, при которой, например, юнит тест будет падать
    [^4]: Перейдя в раздел _ПР_ и выбрав нужный ПР, можно убедиться, что мерж будет заблокирован, если флоу `PR` не был успешен
    [^5]: Запушьте новый коммит с исправленной ошибкой. Проверки пройдут снова, а результаты и блокировка будут пересмотрены
    [^6]: Перейдите в раздел _ПР_, выберите нужный ПР. Вы увидите результаты проверок, а так же можно их посмотреть подробно в подразделе `Checks`
    [^7]: Запуште релизный тэг и в разделе _Actions_ увидите запуск флоу `Release`
    [^8]: В разделе _Issue_, выбрав нужный issue можно увидеть всю необходимую информацию
    [^9]: Успешный отчет или же отчет об ошибках выводится в комментариях в конкретном issue
    [^10]: Можете перейти в настройки, и в разделе _Pages_ будет ссылка на деплой
    [^11]: Issue будет автоматически закрыто с последним сообщением об успешном деплое
    [^12]: С помощью, например, комбинации команд `git tag -f` `git push --force origin v<число>` или любым другим способом можете убедиться в корректной работе флоу
