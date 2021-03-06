# Управление хостами {{ ZK }}

Кластер {{ mch-name }} с одним хостом не отказоустойчив и не обеспечивает репликацию данных. Чтобы кластер был более надежным, можно [добавить дополнительные хосты {{ CH }}](hosts.md), но для управления ими сначала необходимо [включить отказоустойчивость](#add-zk) (при этом в кластер будет добавлено минимально допустимое количество хостов {{ ZK }} — три).

Вы можете [добавлять](#add-zk-host) и [удалять](#delete-zk-host) хосты {{ ZK }} в отказоустойчивом кластере. Всего в отказоустойчивом кластере может быть от трех до пяти хостов {{ ZK }} включительно.

{% note warning %}

Если для кластера уже включена отказоустойчивость и созданы хосты {{ ZK }}, то полностью удалить эти хосты невозможно — в кластере всегда будет минимум три хоста {{ ZK }}. 

{% endnote %}

## Включить отказоустойчивость для кластера {#add-zk}

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ mch-name }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **Хосты**.
  1. Нажмите кнопку **Настроить хосты {{ ZK }}**.
  1. Укажите [класс хостов](../concepts/instance-types.md).
  1. Задайте настройки хранилища.
  1. При необходимости измените настройки хостов {{ ZK }}. Чтобы это сделать, наведите курсор на строку нужного хоста и нажмите значок ![image](../../_assets/pencil.svg).
  1. Нажмите кнопку **Сохранить изменения**.

- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы включить отказоустойчивость для кластера:
  1. Посмотрите описание команды CLI для добавления хостов {{ ZK }}:

     ```
     $ yc managed-clickhouse cluster add-zookeeper --help
     ```

  1. Запустите операцию с характеристиками хостов по умолчанию:

     ```bash
     $ yc managed-clickhouse cluster add-zookeeper <имя кластера> \
                             --host zone-id=ru-central1-c,subnet-name=default-c \
                             --host zone-id=ru-central1-a,subnet-name=default-a \
                             --host zone-id=ru-central1-b,subnet-name=default-b
     ```

     Если в сети, в которой расположен кластер, ровно 3 подсети, по одной в каждой зоне доступности, то явно указывать подсети для хостов необязательно: {{ mch-name }} автоматически распределит хосты по этим подсетям. 

     Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters).

- API

  Воспользуйтесь методом  [addZookeeper](../api-ref/Cluster/addZookeeper.md). При добавлении укажите настройки для трех хостов {{ ZK }}, перечислив их в параметре `hostSpecs`.

{% endlist %}

{% note info %}

По умолчанию для хостов {{ ZK }} задаются следующие характеристики:
* класс хоста `b2.medium`;
* объем диска 10 ГБ;
* [тип хранилища](../concepts/storage.md) — быстрое сетевое.

{% endnote %}

## Добавить хост {{ ZK }} {#add-zk-host}

{% list tabs %}

- Консоль управления
  
  1. Перейдите на страницу каталога и выберите сервис **{{ mch-name }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **Хосты**.
  1. Нажмите кнопку **Добавить хосты {{ ZK }}**.
  1. При необходимости измените настройки хоста.
  1. Нажмите кнопку **Сохранить изменения**.
  
- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы добавить хост в кластер:
  1. Соберите необходимую информацию:
     - Запросите идентификатор подсети, выполнив команду:
       ```
       $ yc vpc subnet list
       ```
       Если нужной подсети в списке нет, [создайте ее](../../vpc/operations/subnet-create.md).
     - Запросите имя кластера со [списком кластеров в каталоге](cluster-list.md#list-clusters).
     
  1. Посмотрите описание команды CLI для добавления хостов:
  
     ```
     $ yc managed-clickhouse host add --help
     ```
     
  1. Выполните команду добавления хоста {{ ZK }}:
  
     ```
     $ yc managed-clickhouse hosts add \
          --cluster-name <имя кластера> \
          --host zone-id=<зона доступности>,subnet-id=<идентификатор подсети>,type=zookeeper
     ```      

- API

  Воспользуйтесь методом [addHosts](../api-ref/Cluster/addHosts.md) и передайте в запросе:
  - Идентификатор кластера, в котором нужно разместить хост, в параметре `clusterId`. Чтобы узнать идентификатор, получите [список кластеров в каталоге](cluster-list.md#list-clusters).
  - Настройки для хоста в параметре `hostSpecs` (в том числе укажите тип `ZOOKEEPER` в параметре `hostSpecs.type`). Не указывайте настройки для нескольких хостов в этом параметре — хосты {{ ZK }} добавляются в кластер по одному, в отличие от [хостов {{ CH }}](hosts.md#add-host), которых можно добавить сразу несколько. 

{% endlist %}

## Удалить хост {{ ZK }} {#delete-zk-host}

{% list tabs %}

- Консоль управления
  
  1. Перейдите на страницу каталога и выберите сервис **{{ mch-name }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **Хосты**.
  1. Наведите курсор на строку нужного хоста и нажмите значок ![image](../../_assets/cross.svg).
  1. Подтвердите удаление хоста.  
  
- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы удалить хост из кластера, выполните команду:
  
  ```
  $ yc managed-clickhouse hosts delete <имя хоста> \
       --cluster-name=<имя кластера>
  ```

  Имя хоста можно запросить со [списком хостов в кластере](hosts.md#list-hosts), имя кластера — со [списком кластеров в каталоге](cluster-list.md#list-clusters).

- API

  Воспользуйтесь методом [deleteHosts](../api-ref/Cluster/deleteHosts.md) и передайте в запросе:
  - Идентификатор кластера, в котором находится хост, в параметре `clusterId`. Чтобы узнать идентификатор, получите [список кластеров в каталоге](cluster-list.md#list-clusters).
  - Имя хоста в параметре `hostNames`. Чтобы узнать имя, получите [список хостов в кластере](hosts.md#list-hosts).

{% endlist %}