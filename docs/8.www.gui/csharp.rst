C#, обращение к REST API
========================

.. seealso::

    * https://github.com/iitwebdev/CSharpYandexAPI
    * https://tech.yandex.ru/translate/doc/dg/concepts/About-docpage/

Пример online-переводчика, который использует класс
`WebRequest <https://msdn.microsoft.com/ru-ru/library/system.net.webrequest(v=vs.110).aspx>`_
для обращения к API Яндекс.Переводчика.

.. seealso::

   `Как делать HTTP запросы на C# <http://lecturesnet.readthedocs.io/net/requests/csharp.html>`_

Для удобства в программе используется простой GUI интерфейс.

.. image:: /_static/8.www.gui/csharp_restapi.png
    :align: center

Введение
--------

Онлайн переводчик, в первую очередь, это программа, позволяющая
переводить текст с иностранного языка и наоборот, в режиме онлайн.
Перевод может быть абсолютно разным, будь-то это фраза,
словосочетание, либо большой и длинный литературный текст. Также
онлайн переводчик может служить помощником в переписке с зарубежными
друзьями или коллегами, в переводе каких-либо иностранных статей. В
конце концов, переводчик может быть полезен в том случае, когда Вы
пытаетесь что-то кому-то сказать на иностранном языке.


Описание
--------

Для создания программы-переводчика использовалось приложение `Visual
Studio 2013`, а также дополнительный фреймворк `Json.NET`.

Приложение написано на `WindowsForms`, на языке `C#`. Для создания
дизайна приложения была использована программа Adobe Photoshop CS5.

С помощью данного приложения можно перевести текст на 9 различных
языков: Английский, Русский, Иврит, Испанский, Итальянский, Китайский,
Немецкий, Французский, Японский. Перевод осуществляется с помощью
веб-запросов к API Яндекс.Переводчика.

Для перевода текста достаточно ввести исходный текст в первое окно,
указать начальный язык, а также требуемый язык перевода во всплывающих
боксах. Для быстрой смены выбранных языков между собой можно
воспользоваться центральной кнопкой в виде двух стрелок. Как только Вы
выбрали нужные языки и ввели необходимый текст, нажмите кнопку
«Перевод». Переведенный текст отобразится во втором окне.


Получение OAuth ключа
---------------------

Чтобы работать с API Яндекс Переводчика, потребовалось получить
API-ключ(OAuth токен доступа).

OAuth-токен ― это специальный код,
разрешающий доступ к данным конкретного пользователя. Для каждого
пользователя (логина в Яндекс.Директе, от имени которого
осуществляются запросы к API) необходимо получить отдельный токен,
который следует указывать при вызове методов.

Ключ был получен на сайте Яндекс https://tech.yandex.ru/translate/

Разработка приложения
---------------------

Расположение объектов приложения было произведено при помощи
WindowsForms, а затем оформлено при помощи изображений, созданных в
Adobe Photoshop CS5. Код приложения написан на языке программирования
C#.

Приложение состоит из трех классов: Form1, Translate и Translation.

Form1
~~~~~

В классе Form1 описаны объекты приложения и действия над ними.
Приложение состоит из двух кнопок, реализованных через pictureBox;
Двух текстбоксов, а также трех комбобоксов (всплывающие списки). В двух
комбобоксах пользователь осуществляет выбор необходимого языка для
перевода. Еще один комбобокс не отображается на экране, а служит лишь
для осуществления быстрой смены двух выбранных языков между собой. 

При нажатии на кнопку «Перевод» (pictureBox1) происходит вызов функции
TranslateText, которая посылает веб-запрос. Однако, при отправлении
запроса, требуется указать коды языков для перевода, например(“ru-en”
или “it-ru”). Для этого была написана структура switch-case, которая,
исходя из выбранных языков в комбобоксах, составляет необходимую нам
строку с кодами языков.

.. literalinclude:: /../examples/nogui/CSharpYandexAPI/TranslatorForWeb/TranslatorForWeb/Form1.cs
   :language: csharp
   :linenos:

Translate
~~~~~~~~~

Класс Translate описывает веб-запрос к серверам Яндекс.Переводчика и
десериализацию JSON полученного ответа. Запрос к API переводчика
содержит ряд обязательных параметров – это: 

| key=<API-ключ> 
| & text=<переводимый текст> 
| & lang=<направление перевода>. 

Необязательные параметры, такие как формат и опции перевода, в
приложении не используются.

Так как ответ приходит в виде структуры данных JSON, требуется его
распарсить. Для этого был установлен дополнительный фреймворк
``Json.NET`` и подключена библиотека ``Newtonsoft.Json``.

.. literalinclude:: /../examples/nogui/CSharpYandexAPI/TranslatorForWeb/TranslatorForWeb/Translate.cs
   :language: csharp
   :linenos:

Translation
~~~~~~~~~~~

Данных класс описывает структуру JSON-ответа на запрос к API
переводчика.
