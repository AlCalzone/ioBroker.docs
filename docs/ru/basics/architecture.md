---
title: архитектура
lastChanged: 13.09.2018
translatedFrom: de
translatedWarning: Если вы хотите отредактировать этот документ, удалите поле «translationFrom», в противном случае этот документ будет снова автоматически переведен
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/ru/basics/architecture.md
hash: tRt6MnfQ7NhW3HVZYiIHrRBTqLWlqKrd6ES9IOnC1ic=
---
# Структура системы
?> ***Это заполнитель*** .<br><br> Помогите с ioBroker и расширьте эту статью. Обратите внимание на [Руководство по стилю ioBroker](https://www.iobroker.net/#de/documentation/community/styleguidedoc.md), чтобы изменения можно было легко применить.

## Архитектура
ioBroker является модульным, т.е. состоит из множества отдельных компонентов. У каждого модуля есть конкретная задача. Чтобы отслеживать события, ioBroker имеет центрального координатора для всех своих модулей. Этим координатором является `js-controller`, работающий в фоновом режиме. Он отвечает за централизованное хранение данных, а также за управление и связь между всеми модулями. Сами модули называются `Adapter`. Адаптеры устанавливаются пользователем только при необходимости. Веб-интерфейс администрирования `admin` сам по себе также является адаптером. Адаптер администратора или для краткости «Admin» - это интерфейс управления системой ioBroker. Администратору обычно звонят по адресу [http:// локальный: 8081](http://localhost:8081).

Когда новый адаптер устанавливается с помощью администратора, файлы адаптера сначала загружаются из Интернета и записываются на жесткий диск сервера. Если адаптер должен быть запущен, сначала создается `Instanz` адаптера. Каждый экземпляр адаптера можно настроить индивидуально, а также остановить и запустить независимо с администратором. Таким образом, каждый экземпляр запускается в собственном процессе, который в фоновом режиме взаимодействует с js-контроллером ioBroker.

В системе `Multihost` с несколькими серверами ioBroker экземпляры адаптеров также могут быть распределены на разных серверах. Это означает, что нагрузка может быть распределена или дополнительное оборудование может быть подключено непосредственно на месте (например, порты ввода-вывода, USB).

Связь между адаптерами, js-контроллерами, базами данных и веб-интерфейсами осуществляется через несколько соединений TCP / IP. Обмен данными происходит либо в виде обычного текста, либо в зашифрованном виде, в зависимости от выбранной настройки.

ioBroker и адаптер в основном написаны на языке программирования JavaScript. Для запуска JavaScript требуется соответствующая среда выполнения. Таким образом, ioBroker основан на `Node.js`. Эта среда выполнения доступна для широкого спектра программных платформ, таких как Linux, Windows и macOS. Диспетчер пакетов JavaScript `npm` используется для установки ioBroker и адаптеров.

@@@ Пояснение красивой картинки с архитектурными слоями @@@ @@@ JS-контроллером и переходом на адаптеры и экземпляры @@@