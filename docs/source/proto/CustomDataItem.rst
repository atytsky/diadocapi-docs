CustomDataItem
==============

Структура ``CustomDataItem`` представляет собой :doc:`тег документа <../entities/tag>`.

.. code-block:: protobuf

    message CustomDataItem
    {
        required string Key = 1;
        optional string Value = 2;
    }

- ``Key`` — название тега. Не может быть пустым. Регистронезависимый. Может содержать только буквы латинского и русского алфавита, цифры и символы «-» и «_». Длина не может превышать 60 символов.
- ``Value`` — значение тега, соответствующее ключу ``Key``. Может быть пустым. Регистронезависимый. Длина не может превышать 200 символа.

----

.. rubric:: Смотри также

*Руководства:*
	- :doc:`../entities/tag`

*Структура используется:*
	- в структуре :doc:`DocumentInfoV3`,
	- в структуре :doc:`Document`,
	- в структуре :doc:`obsolete/Docflow`,
	- в структуре :doc:`DocumentAttachment`,
	- в структуре :doc:`TemplateDocumentAttachment`.