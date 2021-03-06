# Получение списка алиасов

 Для получения списка алиасов вам необходимо знать уникальный идентификатор или имя реестра. Как узнать уникальный идентификатор или имя реестра, читайте в разделе [{#T}](../../registry/registry-list.md).

{% list tabs %}

- Консоль управления

   Чтобы просмотреть список алиасов:

   1. В [консоли управления](https://console.cloud.yandex.ru/) выберите каталог, в котором вы хотите получить список алиасов.
   1. Выберите сервис **{{ iot-name }}**.
   1. Выберите в списке нужный реестр.
   1. В левой части окна выберите раздел **Устройства**.
   1. Выберите в списке нужное устройство.
   1. На странице **Обзор** перейдите к разделу **Алиасы**.

- CLI
    
    {% include [cli-install](../../../../_includes/cli-install.md) %}
    
    {% include [default-catalogue](../../../../_includes/default-catalogue.md) %}
    
    Список алиасов можно получить только для всех устройств в реестре.
    
    Получите список алиасов для всех устройств в реестре:
    
    ```
    $ yc iot registry list-device-topic-aliases my-registry
    +----------+----------------------------------------+----------------------+
    |  ALIAS   |              TOPIC PREFIX              |      DEVICE ID       |
    +----------+----------------------------------------+----------------------+
    | commands | $devices/arenak5ciqss6pbas6js/commands | arenak5ciqss6pbas6js |
    +----------+----------------------------------------+----------------------+
    ```
    
{% endlist %}
