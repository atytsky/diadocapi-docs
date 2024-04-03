SendFnsRegistrationMessage
==========================

Метод ``SendFnsRegistrationMessage`` отправляет заявление участника ЭДО для ящика.

.. http:post:: /SendFnsRegistrationMessage

	:queryparam boxId: идентификатор :doc:`ящика <../entities/box>` организации.

	:requestheader Authorization: данные, необходимые для :doc:`авторизации <../Authorization>`.

	:request Body: Тело запроса должно содержать список сертификатов, представленный структурой ``FnsRegistrationMessageInfo``:

		.. code-block:: protobuf

		    message FnsRegistrationMessageInfo
		    {
		        repeated bytes Certificates = 1;
		    }

		..

		- ``Certificates`` — список новых сертификатов, которые ранее не были зарегистрированы в ФНС, сериализованные в массивы байтов в `DER <http://www.itu.int/ITU-T/studygroups/com17/languages/X.690-0207.pdf>`__-кодировке.
	
	:statuscode 200: операция успешно завершена.
	:statuscode 400: данные в запросе имеют неверный формат или отсутствуют обязательные параметры.
	:statuscode 401: в запросе отсутствует HTTP-заголовок ``Authorization`` или в этом заголовке содержатся некорректные авторизационные данные.
	:statuscode 402: у организации с указанным идентификатором ``boxId`` закончилась подписка на API.
	:statuscode 403: доступ к ящику с предоставленным авторизационным токеном запрещен.
	:statuscode 405: используется неподходящий HTTP-метод.
	:statuscode 409: в свойствах организации не указаны поля: ОГРН, код ИФНС, код региона.
	:statuscode 500: при обработке запроса возникла непредвиденная ошибка.

В заявлении будут отправлены все действующие сертификаты, которые были добавлены ранее, и сертификаты, переданные в поле ``FnsRegistrationMessageInfo.Certificates``.

Данные для заявления в ФНС будут взяты из реквизитов организации.
	
Метод не позволяет отправить заявление для тестовой организации.
