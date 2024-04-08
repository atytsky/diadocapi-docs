Register
========

Метод ``Register`` выполняет следующие действия:

	- находит или создает в Диадоке организацию по :doc:`сертификату <../entities/certificate>`,
	- добавляет владельца сертификата в сотрудники этой организации или создает запрос на доступ.

.. http:post:: /Register

	:requestheader Authorization: данные, необходимые для :doc:`авторизации <../Authorization>`.

	:request Body: Тело запроса должно содержать структуру :doc:`../proto/RegistrationRequest`.

	:statuscode 200: операция успешно завершена.
	:statuscode 400: данные в запросе имеют неверный формат или отсутствуют обязательные параметры.
	:statuscode 401: в запросе отсутствует HTTP-заголовок ``Authorization`` или в этом заголовке содержатся некорректные авторизационные данные.
	:statuscode 405: используется неподходящий HTTP-метод.
	:statuscode 500: при обработке запроса возникла непредвиденная ошибка.

	:response Body: Тело ответа содержит структуру :doc:`../proto/RegistrationResponse`.

Поиск и создание организации
	Метод выполняет поиск организации в Диадоке по ИНН из сертификата. Если организации нет в Диадоке, для нее будет создан ящик на основе данных ЕГРЮЛ/ЕГРИП.

Добавление сотрудника
	Добавление сотрудника происходит согласно :ref:`правилам авторегистрации сотрудника <autoregistration>`.
	В процессе регистрации может потребоваться подтверждение владения закрытым ключом сертификата с помощью метода :doc:`RegisterConfirm`.

Примеры использования
---------------------

Примеры запросов
^^^^^^^^^^^^^^^^

С телом сертификата
~~~~~~~~~~~~~~~~~~~

.. sourcecode:: http

    POST /Register HTTP/1.1
    Host: diadoc-api.kontur.ru
    Authorization: DiadocAuth ddauth_api_client_id=key, ddauth_token=token
    Content-Type: application/json; charset=utf-8

    {
        "CertificateContent": "<certificateBytesBase64>"
    }

С отпечатком сертификата
~~~~~~~~~~~~~~~~~~~~~~~~

.. sourcecode:: http

    POST /Register HTTP/1.1
    Host: diadoc-api.kontur.ru
    Authorization: DiadocAuth ddauth_api_client_id=key, ddauth_token=token
    Content-Type: application/json; charset=utf-8

    {
        "Thumbprint": "B0E10292B0024E7197A76B741CE0D271FEAD7E10"
    }

Примеры ответов
^^^^^^^^^^^^^^^

Регистрация завершилась созданием сотрудника
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
        "RegistrationStatus": "RegistrationIsCompleted",
        "BoxId": "99a4b249-a144-4f11-9d7b-4d84fdb68f8b"
    }

Требуется подтвердить владение закрытым ключом
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8

    {
        "RegistrationStatus": "CertificateOwnershipProofIsRequired",
        "BoxId": "2d81936a-d304-4b70-83bc-ab964e7a0f60",
        "DataToSign": "<bytesBase64>"
    }

Пример с использованием C# SDK
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: csharp

    var request = new RegistrationRequest
    {
        Thumbprint = certificate.Sha1Thumbprint
    };

    var response = api.Register(token, request);
        
    if (response.RegistrationStatus == RegistrationStatus.CertificateOwnershipProofIsRequired)
    {
        api.RegisterConfirm(
            token,
            new RegistrationConfirmRequest
            {
                Thumbprint = certificate.Sha1Thumbprint,
                DataToSign = response.DataToSign,
                Signature = Sign(response.DataToSign, certificate)
            });
            
         response = api.Register(token, request);
    }
    
    if (response.RegistrationStatus == RegistrationStatus.RegistrationIsInProcess)
    {
        Thread.Sleep(TimeSpan.FromSeconds(5));
        response = api.Register(token, request);
    }
    
    Console.WriteLine(string.Format("BoxId: {0}, Status: {1}", response.BoxId, response.RegistrationStatus);
