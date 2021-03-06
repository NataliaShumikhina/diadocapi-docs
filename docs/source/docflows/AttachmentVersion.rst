Форматы документов
==================

Формат №155
-----------

Формат, утвержденный Приказом ФНС России от `24.03.2016 №ММВ-7-15/155@ <https://normativ.kontur.ru/document?moduleId=1&documentId=339569>`__, позволяет сформировать 3 разных типа документов:

- *счета-фактуры*,

- *накладные, акты, другие первичные документы*,

- *УПД*.

В XML-файле документа необходимо заполнить поле «функция», которая определит тип документа:

- *счет-фактура* ( *СЧФ* ),

- *первичный документ* ( *ДОП* ),

- *УПД* ( *СЧФДОП* ).

Формат № 820
------------

Формат №820, утвержденный `Приказом ФНС России от 19.12.2018 № ММВ-7-15/820@ <https://normativ.kontur.ru/document?moduleId=1&documentId=328588&cwi=517>`__, пришел на смену формату №155 и распространяется на те же типы документов:

- *счета-фактуры*,
- *накладные, акты и другие первичные документы*,
- *УПД*.

.. note::
    До конца 2019 года действующими являются оба формата - №155 и №820, с 1 января 2020 года формат №155 утратит свою силу.

.. rubric:: Работа в Диадоке

Для Диадока документ в формате №155 или №820 – это контейнер, внутри которого лежит информация о сделке, совершенной клиентом.

Диадок ничего не знает о том, как выглядит учетная политика клиентов, и, соответственно, не может угадать дали ему *ЭСФ/АКТ/Накладную/УПД*.

Более того, по содержанию документа так же нельзя однозначно отличить один тип документа от другого.

.. note::
    При отправке документа участник ЭДО всегда сам выбирает показатель «тип документа».

.. rubric:: Пример

Если продавец выставит счет-фактуру по формату согласно Приказу №155 или Приказу №820, укажет в документе функцию СЧФ, но тип документа в API укажет *UniversalTransferDocument*, то Диадок визуализирует документ, как УПД.

Чтобы визуализация счета-фактуры была привычной, при отправке документа необходимо указывать тип в API *Invoice*.

.. rubric:: Особенности и ограничения

Есть различия в работе с форматами №155 и №820, они касаются генерации и парсинга документов.

Для генерации документов в формате №155 можно использовать универсальные методы генерации `GenerateSenderTitleXml <http://api-docs.diadoc.ru/ru/latest/http/GenerateSenderTitleXml.html>`_ и `GenerateRecipientTitleXml <http://api-docs.diadoc.ru/ru/latest/http/GenerateRecipientTitleXml.html>`_, а также специальные методы для документов в формате УПД `GenerateUniversalTransferDocumentXmlForSeller <http://api-docs.diadoc.ru/ru/latest/http/utd/GenerateUniversalTransferDocumentXmlForSeller.html>`_ и `GenerateUniversalTransferDocumentXmlForBuyer <http://api-docs.diadoc.ru/ru/latest/http/utd/GenerateUniversalTransferDocumentXmlForBuyer.html>`_.

Для парсинга документов в формате №155 можно использовать универсальный метод `ParseTitleXml <http://api-docs.diadoc.ru/ru/latest/http/ParseTitleXml.html>`_, а также специальные методы для документов в формате УПД `ParseUniversalTransferDocumentSellerTitleXml <http://api-docs.diadoc.ru/ru/latest/http/utd/ParseUniversalTransferDocumentSellerTitleXml.html>`_ и `ParseUniversalTransferDocumentBuyerTitleXml <http://api-docs.diadoc.ru/ru/latest/http/utd/ParseUniversalTransferDocumentBuyerTitleXml.html>`_.

Для генерации и парсинга документов в формате №820 можно использовать только универсальные методы:
`GenerateSenderTitleXml <http://api-docs.diadoc.ru/ru/latest/http/GenerateSenderTitleXml.html>`_ и `GenerateRecipientTitleXml <http://api-docs.diadoc.ru/ru/latest/http/GenerateRecipientTitleXml.html>`_ для генерации,
`ParseTitleXml <http://api-docs.diadoc.ru/ru/latest/http/ParseTitleXml.html>`_ для парсинга


.. csv-table:: Соответствие типов документов и форматов документов
   :header: "Веб", "API", "Форматы", "Функция", "Печатная форма"
   :widths: 10, 10, 10, 10, 10

   "СФ", "Invoice", "- приказ №93

   - приказ №155
   - приказ №820", "- –
   - СЧФ", "СФ"
   "КСФ", "InvoiceCorrection", "- приказ №93

   - приказ №189", "- –
   - КСЧФ", "КСФ"
   "Накладная", "XmlTorg12", "- приказ №172

   - приказ №155
   - приказ №820
   - приказ №551", "- –
   - ДОП
   - –", "Накладная"
   "Акт", "XmlAcceptanceCertificate", "- приказ №172

   - приказ №155
   - приказ №820
   - приказ №552", "- –
   - ДОП
   - –", "Акт"
   "УПД", "UniversalTransferDocument", "- приказ №155
   
   - приказ №820", "- СЧФ
   - ДОП
   - СЧФДОП", "УПД"
   "УКД", "UniversalCorrectionDocument", "- приказ №189", "- КСЧФ
   - ДИС
   - КСЧФДИС", "УКД"

Возможные форматы
-----------------

В связи с тем, что документ может быть в разных форматах – интеграционным решениям необходимо понимать в каком формате пришел документ, что бы корректно обработать его на своей стороне.

Для получения акутальной информации о XSD-схеме документа введено специальное поле *Version*. Оно есть в структурах данных :doc:`Document <../proto/Document>`, :doc:`Entity <../proto/Entity message>` и :doc:`DocumentInfo <../proto/DocumentInfo>`.

.. note::
    Ниже приведен неполный список версий документов. Актуальные версии документа следует получать с помощью метода :doc:`GetDocumentTypes <../http/GetDocumentTypes>`

.. csv-table:: Примеры типов и значений Version для формализованных документов
   :header: "Тип документы", "Структура", "Возможные версии"
   :widths: 10, 10, 10

   "Счет-фактура (СФ)", "Invoice", "- invoice_05_01_01
   - invoice_05_01_03
   - invoice_05_02_01
   - utd_05_01_01
   - utd_05_01_02
   - utd_05_01_04
   - utd_05_01_05
   - utd_05_02_01
   - utd820_05_01_01"
   "Исправление СФ", "InvoiceRevision", "- invoice_05_01_03
   - invoice_05_02_01
   - utd_05_01_01
   - utd_05_01_02
   - utd_05_01_04
   - utd_05_01_05
   - utd_05_02_01
   - utd820_05_01_01"
   "Корректировочный СФ (КСФ)", "InvoiceCorrection", "- invoicecor_05_01_03
   - invoicecor_05_02_01
   - ucd_05_01_01
   - ucd_05_01_02
   - ucd_05_02_01"
   "Исправление КСФ", "InvoiceCorrectionRevision", "- invoicecor_05_01_03
   - invoicecor_05_02_01
   - ucd_05_01_01
   - ucd_05_01_02
   - ucd_05_02_01"
   "Формализованный ТОРГ-12", "XmlTorg12", "- torg12_05_01_01
   - torg12_05_01_02
   - utd_05_01_01
   - utd_05_01_02
   - utd_05_01_04
   - utd_05_01_05
   - utd_05_02_01
   - utd820_05_01_01
   - tovtorg_05_01_02
   - tovtorg_05_01_03
   - tovtorg_05_02_01"
   "Формализованный акт", "XmlAcceptanceCertificate", "- act_05_01_01
   - act_05_01_02
   - utd_05_01_01
   - utd_05_01_02
   - utd_05_01_04
   - utd_05_01_05
   - utd_05_02_01
   - utd820_05_01_01
   - rezru_05_01_01
   - rezru_05_02_01"
   "УПД", "UniversalTransferDocument", "- utd_05_01_01
   - utd_05_01_02
   - utd_05_01_04
   - utd_05_01_05
   - utd_05_02_01
   - utd820_05_01_01"
   "Исправление УПД", "UniversalTransferDocumentRevision", "- utd_05_01_01
   - utd_05_01_02
   - utd_05_01_04
   - utd_05_01_05
   - utd_05_02_01
   - utd820_05_01_01"
   "УКД", "UniversalCorrectionDocument", "- ucd_05_01_01
   - ucd_05_01_02
   - ucd_05_02_01"
   "Исправление УКД", "UniversalCorrectionDocumentRevision", "- ucd_05_01_01
   - ucd_05_01_02
   - ucd_05_02_01"

.. important::
  ``AttachmentVersion = UniversalTrnsaferDocument`` для СФ/ИСФ и ``AttachmentVersion = UniversalCorrectionDocument`` для КСФ/ИКСФ считаются устаревшими. Поле AttachmentVersion устарело. Вместо него используйте Version.

.. csv-table:: Типы и значения Version для неформализованных документов
    :header: "Тип документы", "Структура", "Возможные версии"
    :widths: 10, 10, 10

    "Неформализованный документ", "Nonformalized", "v1"
    "Приглашение к ЭДО", "TrustConnectionRequest", "v1"
    "Неформализованный ТОРГ-12", "Torg12", "v1"
    "Неформализованный акт", "AcceptanceCertificate", "v1"
    "Счет", "ProformaInvoice", "v1"
    "Ценовой лист", "PriceList", "v1"
    "Протокол согласования цены", "PriceListAgreement", "v1"
    "Реестр сертификатов", "CertificateRegistry", "v1"
    "Акт сверки", "ReconciliationAct", "v1"
    "Договор", "Contract", "v1"
    "Накладная", "Torg13", "v1"
    "Детализация", "ServiceDetails", "v1"
    "Доп. соглашение", "SupplementaryAgreement", "v1"

.. rubric:: Добавление новых версий

При обновление форматов формализованных документов ФНС, в Диадоке будут добавляться новые значения *Version*, соответствующие новым версиям формата.

Интеграционным решениям нужно быть готовыми к тому, что может прийти новое значение *Version*. Рекомендуется уметь обрабатывать такие ситуации.
