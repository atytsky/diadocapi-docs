DetectDocumentTitles
====================

Метод ``DetectDocumentTitles`` определяет возможные типы указанного документа.

.. http:post:: /DetectDocumentTitles

	:queryparam boxId: идентификатор ящика организации.

	:request Body: Тело запроса должно содержать бинарные данные документа.

.. http:get:: /DetectDocumentTitles

	:queryparam boxId: идентификатор ящика организации.
	:queryparam nameOnShelf: имя файла на :doc:`полке документов<../entities/shelf>`.

	:requestheader Authorization: данные, необходимые для :doc:`авторизации <../Authorization>`.

	:statuscode 200: операция успешно завершена.
	:statuscode 401: в запросе отсутствует HTTP-заголовок ``Authorization`` или в этом заголовке содержатся некорректные авторизационные данные.
	:statuscode 402: у организации с указанным идентификатором ``boxId`` закончилась подписка на API.
	:statuscode 403: доступ к ящику с предоставленным авторизационным токеном запрещен.
	:statuscode 404: не найден ящик с указанным идентификатором.
	:statuscode 500: при обработке запроса возникла непредвиденная ошибка.

	:response Body: Тело ответа содержит описание типов документов, представленное структурой ``DetectTitleResponse``:

		.. code-block:: protobuf

			message DetectTitleResponse {
				repeated DetectedDocumentTitle DocumentTitles = 1;
			}
			
			message DetectedDocumentTitle {
				required string TypeNamedId = 1;
				required string Function = 2;
				required string Version = 3;
				required int32 TitleIndex = 4;
				repeated Events.MetadataItem Metadata = 5;
			}

		..

Метод можно использовать в двух вариантах:

    - ``POST`` запрос с заполненным ``Request Body``,
    - ``GET`` запрос с параметром ``nameOnShelf``, если содержимое документа было загружено на полку методом :doc:`../http/ShelfUpload`.
	
.. note::
	Метод будет определять только те типы документов, которые доступны в текущей организации.
