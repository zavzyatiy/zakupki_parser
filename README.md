## Общая информация

Данный парсер предназначен для формирования пула закупочных процедур, используемых ФАС России для обвинения по делам о недобросовестной конкуренции, из открытых источников и добыче всей необходимой информации для работы с ними. В этом проекте представлен не только сам код, который может быть использован для своей задачи, но и оригинальный ноутбук автора для решения его задачи, на который можно опираться в качестве _примера_.

Это первая версия данного проекта, обновленная последний раз 05.05.2024, предназначенная для парсинга **только** электронных аукционов, проходивших **исключительно** на площадке Сбербанк-АСТ.

К сожалению, работа программы в некоторых местах предполагает ручные действия, но в целом, даже на данном этапе удалось добиться достаточной автоматизации сбора данных. Пояснение к проекту даны ниже.

## Структура проекта и некоторые комментарии

- Предполагается, что для работы на своем устройстве пользователь загрузит из проекта всю папку root на Рабочий стол (если использует ОС Windows; если нет, то тут автор бессилен).
- Все архивы и ноутбук zakupki.ipynb представляют из себя образец данных, собранных автором, оригинального кода, их обработавшего, и выходных данных и используются для демонстрации результатов его работы. Код, приспособленный для форматирования - это ноутбук zakupki_for_scaling.ipynb. Чтобы работать в нем необходимо изменить DESKTOP_USER_NAME на имя пользователя на своем копьютере (в разделе "Создание таблиц с нужными данными" в самой первой вкладке "Функции" в первой ячейке).
- Все ячейки кода необходимо запускать последовательно или несколько раз (об этом подробнее ниже).
- На компьютере необходимо иметь браузер Хром и скаченное в нем расширение EditThisCookie (<a href="https://chromewebstore.google.com/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg?pli=1" target="_blank">ссылка</a>).
- Формальное описание всей процедуры сбора данных есть во втором разделе курсовой работы, оставленной выше.

## Алгоритм работы с парсером

1. Рассмотрим некоторое решение ФАСа со списком фирм, означенных как картель. В данном коде рассмотрено дело № 4-14.32-402/00-30-17 (но он способен работать и с другими). Из дела нужно взять помимо этого коды ОКДП (если есть; иначе подбирать самому, как в моем случае), даты, во время которых реализовывалось соглашение, и список конкурентных фирм, участвовавших в закупках вместе с фирмами-участниками картеля (если нет, то рассмотреть торги без фирм-участников картеля и верить в то, что другие все конкурентные). В целом, если нет нужды в сравнительном анализе, то конкурентные фирмы не нужны.

2. Необходимо найти ИНН всех фирм. Можно это делать вручную или с использованием API СПАРКа (такое вроде есть, но там нужен корпоративный доступ). Результаты этой процедуры лежат в carteling.txt и competing.txt .

3. На ЕИС (https://zakupki.gov.ru/) ищем закупки с нужными параметрами (ФЗ, дата, площадка: Сбербанк-АСТ и т.д.) и вставляем ИНН фирм из картеля из пункта (2). Дальше выгружаем всю информацию в xlsx формате, если закупок за дату больше 5000, то дробим по дате так, чтобы скачать в несколько xlsx всё в итоге. Файлы называем числами, начиная с 1. Все эти файлы сохраняем в папку to_parse.

4. Теперь переходим к коду. Проигрываем ячейки в "Библиотеки", "Функции", последовательно воспроизводим ячейки из "Общие таблицы с выгрузкой | Картель". 

5. Далее открываем окно в Хроме и копируем cookie файлы со страницы любой закупки на сайте Сбербанк-АСТ (например, здесь https://www.sberbank-ast.ru/ViewDocument.aspx?id=299236046). Копировать следует в ячейку кода в разделе "Таблицы с участниками | Картель", которая начинается, как content = """... Вставить скопированный текст необходимо между """ """, как показано в примере. После этого нужно последовательно проиграть все ячейки в этом же разделе. Самая важная и долгая из них - та, перед которой написан комментарий.

6. Разделы кода, названные в конце "... | Конкурентные", устроены так же и работаю аналогично.

## Что можно улучшить?

- да примерно все :)
