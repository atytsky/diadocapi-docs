GetDepartment
=============

.. note::
	Вызов метода доступен только администраторам организации.

Метод ``GetDepartment`` возвращает информацию о подразделении организации.

.. http:get:: /admin/GetDepartment

	:queryparam boxId: идентификатор :doc:`ящика <../../entities/box>` организации.
	:queryparam departmentId: идентификатор подразделения организации.

	:requestheader Authorization: данные, необходимые для :doc:`авторизации <../../Authorization>`.

	:statuscode 200: операция успешно завершена.
	:statuscode 400: данные в запросе имеют неверный формат или отсутствуют обязательные параметры.
	:statuscode 401: в запросе отсутствует HTTP-заголовок ``Authorization`` или в этом заголовке содержатся некорректные авторизационные данные.
	:statuscode 402: у организации с указанным идентификатором ``boxId`` закончилась подписка на API.
	:statuscode 403: доступ к ящику с предоставленным авторизационным токеном запрещен или запрос сделан не от имени администратора.
	:statuscode 404: в указанном ящике нет подразделения с указанным идентификатором.
	:statuscode 405: используется неподходящий HTTP-метод.
	:statuscode 500: при обработке запроса возникла непредвиденная ошибка.

	:response Body: Тело ответа содержит информацию о подразделении, представленную структурой :doc:`../../proto/Departments/Department`.

SDK
"""

.. note::
	В SDK соответсвующий метод имеет название ``GetDepartmentByFullId``. Метод ``GetDepartment`` в SDK соответствует методу API :doc:`../../http/GetDepartment`.

Пример использования (C#)
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: csharp

    var department = api.GetDepartmentByFullId(token, boxId, departmentId);