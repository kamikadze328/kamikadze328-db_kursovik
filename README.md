## Информационная система
### API довольно простое:  
-	Для получения информации с сервера делается запрос к скрипту с ключём, определяющим данный. Например, чтобы получить список всех поломок нужно отправить на /php/get_data.php post запрос с {purpose: “breakdowns”}. И так для всей нужной информации. 
-	Для изменения информация на сервере я выделил отдельные скрипты для каждого запроса, потому что не так много что-то изменяющих вопросов.
-	В приложении существует строгая форма ответы пользователю, со внутренным списком кодов ошибок, чтобы всегда было понятно, что пошло не так. На клиент передаётся специально структурированный ответ вида  
{"error" : {"code": $error_code,  "message": $message,  "dbMessage": $db_message}}  
Или  
{"message": $message,  "data": $data,  "code": 200);
-	Коды ошибок:
const CODE_TO_MESSAGE = array(
    WRONG_REQUEST_PARAMS => 'Неверные параметры запроса.',
    DB_ERROR => 'Ошибка базы данных.',
    402 => '',
    DB_NOT_AVAILABLE => 'База данных недоступна.',
    WRONG_PASSWD => 'Пароль неверный',
    405 => '',
    406 => '',
    407 => '',
    USER_NOT_EXISTS => 'Такого пользователя не существует.',
    USER_EXISTS => 'Такой пользователь существует.',
    SMTH_WRONG => 'Что-то пошло не так.',
    CANT_UPDATE_ROLE => 'Эту роль нельзя изменить.',
    CANT_UPDATE_USER => 'Вы не можете изменять этого пользователя.',
    CANT_SET_ROLE => 'Вы не можете назначить эту роль.'
    );

### Стек технологий:
-	PHP8 – до невозможного просто для такой маленькой задачи. Полностью реализуется все мои цели. Правда пришлось столкнуться с проблемой, что на сервере очень старый php и нет никаких библиотек для общения с базой данных, поэтому пришлось перехать на локалку.
-	Vue 3 + Vuex + Router – Vue js мне был известен, поэтому здесь даже выбирать не пришлось. Современный фронтенд фреймворк – идеально подходит для SPA. Vuex и Router в реальной жизни практически всегда используются вместе с Vue.
-	Postgres – работает.
### Архитектура фронт:
SPA с роутингом, что позоволяет минимизировать обращения к серверу, и задействовать все ресурсы пользователя. Приложение хранит свои данные для обращения их внутри фронтенда с помощью Vuex. В общем представляет собой компонентную структуру, со множественным переиспользованием компонентов. Все кликабельные текстовые элементы выделены зелёным, а ссылки имеют стандартный сине-подчёркнутый вид.
### Архитектура бекенд:
В отдельный класс вынесена логика работы с базами данных – DBManager.php. Так же в отдельные класс вынесены коды ошибок и методы работы с ними, и отдельный скрипт с общими функциями – сформировать ответ пользователю нужного формата, прочитать файл… итд. В основная полезная работа происходит в маленьких скриптах, которые просто пользуются уже предоставленными функциями. 
