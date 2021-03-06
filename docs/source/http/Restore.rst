Restore
=======

Имя ресурса: **/Restore**

HTTP метод: **POST**

Параметры строки запроса:

-  *boxId* - идентификатор ящика;

-  *messageId* - идентификатор сообщения;

-  *documentId* - идентификатор документа в сообщении (может отсутствовать).

В запросе должен присутствовать HTTP-заголовок ``Authorization`` с необходимыми данными для :doc:`авторизации <../Authorization>`.

Метод позволяет восстанавливать документы из удаленных. Если параметр *documentId* не задан, то сообщение messageId восстанавливается целиком, и все документы в нем автоматически помечаются как неудаленные.

Когда в удаленном сообщении восстанавливается хотя бы один документ, пометка "удален" снимается и с самого сообщения.

Метод позволяет восстанавливать и шаблоны, для этого в *messageId* нужно передать идентификатор сообщения шаблона.

Для вызова этого метода текущий пользователь должен иметь доступ ко всем восстанавливаемым документам, в противном случае возвращается код ошибки 403 (Forbidden).

Возможные HTTP-коды возврата:

-  200 (OK) - операция успешно завершена;

-  400 (Bad Request) - данные в запросе имеют неверный формат или отсутствуют обязательные параметры;

-  401 (Unauthorized) - в запросе отсутствует HTTP-заголовок ``Authorization``, или в этом заголовке содержатся некорректные авторизационные данные;

-  403 (Forbidden) - доступ к ящику с предоставленным авторизационным токеном запрещен;

-  404 (Not found) - не найдено сообщение/документ с заданным идентификатором;

-  405 (Method not allowed) - используется неподходящий HTTP-метод;

-  409 (Conflict) - попытка повторного восстановления документа/сообщения;

-  500 (Internal server error) - при обработке запроса возникла непредвиденная ошибка.