# Beget Terraform provider: документация

## О провайдере

[Облачные сервисы Beget](https://beget.com/ru/cloud) могут управляться пользователями не только вручную через Панель управления, но и с помощью автоматизированных средств, таких как [Hashicorp Terraform](https://terraform.io). Провайдер для взаимодействия Terraform и нашей системы описан в данной документации.

## Документация

Провайдер Beget для Terraform позволяет использовать параметры, ресурсы и источники данных, перечисленные в подразделах ниже.

### Ресурсы (resource)

* [beget_compute_instance](./docs/resources/compute_instance.md) - виртуальные сервера (VPS);
* [beget_compute_snapshot](./docs/resources/compute_snapshot.md) - снапшоты виртуальных серверов;
* [beget_ssh_key](./docs/resources/ssh_key.md) - SSH-ключи доступа;
* [beget_additional_ip](./docs/resources/additional_ip.md) - дополнительные выделенные IPv4-адреса;
* [beget_private_network](./docs/resources/private_network.md) - приватные сети.

### Источники данных (data source)

* [beget_configuration](./docs/data-sources/configuration.md) - отдельная конфигурация виртуального сервера;
* [beget_configurations](./docs/data-sources/configurations.md) - доступные конфигурации виртуальных серверов;
* [beget_configuration_group](./docs/data-sources/configuration_group.md) - отдельная группа конфигураций виртуальных серверов, связанных по типу процессора;
* [beget_configuration_groups](./docs/data-sources/configuration_groups.md) - доступные группы конфигураций виртуальных серверов, связанных по типу процессора;
* [beget_isp_license](./docs/data-sources/isp_license.md) - отдельный тип лицензии ISP Manager;
* [beget_isp_licenses](./docs/data-sources/isp_licenses.md) - список доступных типов лицензии ISP Manager;
* [beget_private_networks](./docs/data-sources/private_networks.md) - список созданных для аккаунта приватных сетей;
* [beget_region](./docs/data-sources/region.md) - отдельная локация для размещения виртуальных серверов;
* [beget_regions](./docs/data-sources/regions.md) - список доступных локаций для размещения виртуальных серверов;
* [beget_software](./docs/data-sources/software.md) - информация об отдельном дистрибутиве ПО из [маркетплейса](https://beget.com/ru/cloud/marketplace/);
* [beget_softwares](./docs/data-sources/softwares.md) - список доступных для установки дистрибутивов ПО из [маркетплейса](https://beget.com/ru/cloud/marketplace/).

### Настройки провайдера

#### Обязательные

* ``token`` - токен доступа к Beget API, необходимый для работы провайдера.

#### Необязательные

* ``default_create_timeout`` - время таймаута на операции создания ресурсов;
* ``default_delete_timeout`` - время таймаута на операции удаления ресурсов;
* ``default_read_timeout`` - время таймаута на операции получения данных от Beget API;
* ``default_update_timeout`` - время таймаута на операции обновления ресурсов.

## Быстрый старт

### Установка Terraform

Для работы необходимо установить клиент Terraform на устройство, с которого Вы планируете работать. Для этого воспользуйтесь [официальной документацией Terraform](https://developer.hashicorp.com/terraform/tutorials/docker-get-started/install-cli).

### Создание конфигурационного файла

1. Для работы с Terraform рекомендуется создать отдельную директорию, в которой будут располагаться все файлы для работы над проектом. Создайте директорию с произвольным именем (например, ``beget-cloud-project``) и перейдите в нее.
2. В созданной директории создайте конфигурационный файл, в котором будет описан план для Terraform. Файл должен иметь расширение ``.tf``, имя может быть произвольным (например, ``main.tf``).
> *Terraform поддерживает описание планов как на [собственном языке HCL](https://developer.hashicorp.com/terraform/language/syntax/configuration), так и [в формате JSON](https://developer.hashicorp.com/terraform/language/syntax/json). Во втором случае вместо расширения ``.tf`` должно использоваться ``.json``. В данном примере мы будем использовать именно HCL.*

### Подключение провайдера

1. Откройте файл плана Terraform и вставьте в него следующий блок конфигурации:
```
# Содержимое ниже вставьте с заменой в файл main.tf (или создайте его, если он отсутствует)
terraform {
  required_providers {
    beget = {
      source = "tf.beget.com/beget/beget"
    }
  }
}
```
2. В командной строке введите команду ``terraform init`` для скачивания и инициализации провайдера согласно плана. Terraform выполнит необходимые операции:
```
$ terraform init
Initializing the backend...
Initializing provider plugins...
- Finding latest version of tf.beget.com/beget/beget...
- Installing tf.beget.com/beget/beget v0.0.33...
- Installed tf.beget.com/beget/beget v0.0.33 (self-signed, key ID C4AB1984CA80EFC9)
Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://developer.hashicorp.com/terraform/cli/plugins/signing
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

### Получение токена для провайдера

Для работы с Terraform необходимо получить пользовательский токен. Чтобы это сделать, необходимо:

1. Авторизоваться в [Панели управления Beget](https://cp.beget.com).
2. Перейти в раздел [Настройки аккаунта](https://cp.beget.com/settings), открыть категорию [Ограничение доступа](https://cp.beget.com/settings/access) и перейти в подраздел [Beget API](https://cp.beget.com/settings/access/api).
3. В открытом разделе включить пункт "Разрешить аутентификацию по API".
4. Отправить любым удобным способом (с помощью curl, Postman или любого другого клиента) POST-запрос для получения токена [согласно документации API](https://developer.beget.com/#auth-/v1/auth).
5. Полученный в ответе токен (поле ``token`` в полученном json) указать в файле плана Terraform:
```
# Содержимое ниже вставьте с заменой в файл main.tf (или создайте его, если он отсутствует)
terraform {
  required_providers {
    beget = {
      source = "tf.beget.com/beget/beget"
    }
  }
}

provider "beget" {
  # Полученное в запросе значение токена вставьте сюда
  token = "eyJhb..."
}
```

### Подготовка ресурсов для создания

В качестве примера мы опишем создание виртуального сервера (VPS) в следующей конфигурации:

- локация - Россия, Санкт-Петербург (``ru1``);
- линейка CPU - Standard 3+ GHz (``normal_cpu``);
- конфигурация VPS - 1 ядро CPU, 1 ГБ RAM, 10 ГБ SSD;
- устанавливаемое ПО - Ubuntu 24.04 (в примере определяется с помощью источника данных ``beget_software``).

Для создания VPS необходимо указать хотя бы один публичный ключ в формате RSA - информацию о том, как создать такой ключ, можно прочитать [в нашей документации](https://beget.com/ru/kb/how-to/ssh/avtomaticheskaya-ssh-avtorizacziya-po-klyuchu). После создания пары ключей необходимо будет указать путь до ключа в соответствующем ресурсе ``beget_ssh_key``.

```
# Содержимое ниже вставьте в файл main.tf (или создайте его, если он отсутствует)
terraform {
  required_providers {
    beget = {
      source = "tf.beget.com/beget/beget"
    }
  }
}

provider "beget" {
  # Полученное в запросе значение токена вставьте сюда
  token = "eyJhb..."
}

resource "beget_ssh_key" "devops" {
 name = "Test terraform key"
 # Путь до собственного ключа необходимо указать здесь
 public_key = file("/home/user/.ssh/id_rsa.pub")
}

# Данные обо всех доступных регионах можно получить с помощью data source "beget_regions"
data "beget_regions" "regions_list" {
}

# Для получения списка регионов нужно раскомментировать output ниже
# output "regions_list" {
#  value = data.beget_regions.regions_list
# }

# Данные обо всех доступных линейках CPU можно получить с помощью data source "beget_regions"
data "beget_configuration_groups" "config_group_list" {
}

# Для получения списка линеек CPU нужно раскомментировать output ниже
# output "config_list" {
#   value = data.beget_configuration_groups.config_group_list
# }

# Данные обо всех доступных дистрибутивах ПО можно получить с помощью data source "beget_softwares"
data "beget_softwares" "software_list" {
}

# Для получения списка ПО на экран нужно раскомментировать output ниже
# output "software_list" {
#   value = data.beget_softwares.software_list
# }

# Данные о дистрибутиве ПО определяются с помощью data source "beget_software"
# Для этого можно использовать фильтры по данным, которые можно получить из beget_softwares - в данном случае по полю slug
data "beget_software" "ubuntu" {
  slug = "ubuntu-24-04"
}

resource "beget_compute_instance" "test-server" {
  name = "Test Server"
  description = "Сервер, созданный с помощью Terraform"
  hostname = "test-server"
  # Здесь указывается регион, в котором будет создан VPS - его можно выбрать с помощью data source "beget_regions"
  region = "ru1"
  # Здесь указывается конфигурация VPS
  configuration = {
    cpu = 1
    ram_mb = 1 * 1024
    disk_mb = 10 * 1024
    # Здесь указывается линейка CPU - список доступных можно узнать с помощью data source "beget_configuration_groups"
    cpu_class = "normal_cpu"
  }
  image = {
    software = {
      # Для установки ПО необходим его ID - его мы можем получить как с помощью data source "beget_softwares" напрямую, так и с помощью data source "beget_software" на основе фильтров
      id = data.beget_software.ubuntu.id
    }
  }
  access = {
    ssh_keys = [beget_ssh_key.devops.id]
  }
}
```

После создания VPS доступ к ней можно будет получить как по SSH-ключу, сгенерированному ранее, так и по паролю, который придет на электронную почту, привязанную к аккаунту.

### Проверка и запуск плана

После того, как план подготовлен, необходимо:

1. Удостовериться, что план корректен - для этого выполнить валидацию командой ``terraform validate``:
```
$ terraform validate
Success! The configuration is valid.
```
2. Запустить планирование применения конфигурации (необязательно) с помощью команды ``terraform plan`` - она отобразит изменения состояния проекта без применения изменений:
```
$ terraform plan
data.beget_software.ubuntu: Reading...
data.beget_software.ubuntu: Read complete after 0s [name=ubuntu]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # beget_compute_instance.test-server will be created
  + resource "beget_compute_instance" "test-server" {
      + access           = {
          + ssh_keys = [
              + (known after apply),
            ]
        }
      + configuration    = {
          + cpu         = 1
          + cpu_class   = "normal_cpu"
          + disk_mb     = 10240
          + price_day   = (known after apply)
          + price_month = (known after apply)
          + ram_mb      = 1024
          + region      = (known after apply)
        }
      + date_create      = (known after apply)
      + description      = "Сервер, созданный с помощью Terraform"
      + hostname         = "test-server"
      + id               = (known after apply)
      + image            = {
          + software = {
              + id = 6153
            }
        }
      + ip_address       = (known after apply)
      + name             = "Test Server"
      + region           = "ru1"
      + slug             = (known after apply)
      + software_domain  = (known after apply)
      + status           = (known after apply)
      + technical_domain = (known after apply)
    }

  # beget_ssh_key.devops will be created
  + resource "beget_ssh_key" "devops" {
      + fingerprint = (known after apply)
      + id          = (known after apply)
      + name        = "Test terraform key"
      + public_key  = <<-EOT
            ssh-rsa AAAAB...
        EOT
    }

Plan: 2 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.

```
3. Запустить применение изменений командой ``terraform apply``:
```
$ terraform apply
data.beget_software.ubuntu: Reading...
data.beget_software.ubuntu: Read complete after 0s [name=ubuntu]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # beget_compute_instance.test-server will be created
  + resource "beget_compute_instance" "test-server" {
      + access           = {
          + ssh_keys = [
              + (known after apply),
            ]
        }
      + configuration    = {
          + cpu         = 1
          + cpu_class   = "normal_cpu"
          + disk_mb     = 10240
          + price_day   = (known after apply)
          + price_month = (known after apply)
          + ram_mb      = 1024
          + region      = (known after apply)
        }
      + date_create      = (known after apply)
      + description      = "Сервер, созданный с помощью Terraform"
      + hostname         = "test-server"
      + id               = (known after apply)
      + image            = {
          + software = {
              + id = 6153
            }
        }
      + ip_address       = (known after apply)
      + name             = "Test Server"
      + region           = "ru1"
      + slug             = (known after apply)
      + software_domain  = (known after apply)
      + status           = (known after apply)
      + technical_domain = (known after apply)
    }

  # beget_ssh_key.devops will be created
  + resource "beget_ssh_key" "devops" {
      + fingerprint = (known after apply)
      + id          = (known after apply)
      + name        = "Test terraform key"
      + public_key  = <<-EOT
            ssh-rsa AAAAB...
        EOT
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:

```
4. Подтвердить изменения вводом ``yes`` в командной строке и дождаться выполнения операций:
```
  Enter a value: yes

beget_ssh_key.devops: Creating...
beget_ssh_key.devops: Creation complete after 1s [id=27674]
beget_compute_instance.test-server: Creating...
beget_compute_instance.test-server: Still creating... [00m10s elapsed]
beget_compute_instance.test-server: Still creating... [00m20s elapsed]
beget_compute_instance.test-server: Creation complete after 26s [id=f03a3dbf-d1d7-4b5e-bdca-0ac66531acd6]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

После завершения операций VPS будет создана - к ней можно подключиться по данным, указанном в письме, которое было отправлено на электронную почту. Также она будет отображаться в разделе [Облако](https://cp.beget.com/cloud) Панели управления.

### Удаление созданных сервисов
Если в сервисах больше нет необходимости - их можно удалить. Для этого:

1. Запустите удаление командой ``terraform destroy``:
```
$ terraform destroy
beget_ssh_key.devops: Refreshing state... [id=27709]
data.beget_software.ubuntu: Reading...
data.beget_software.ubuntu: Read complete after 0s [name=ubuntu]
beget_compute_instance.test-server: Refreshing state... [id=f03a3dbf-d1d7-4b5e-bdca-0ac66531acd6]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # beget_compute_instance.test-server will be destroyed
  - resource "beget_compute_instance" "test-server" {
      - access           = {
          - ssh_keys = [
              - 27709,
            ] -> null
        } -> null
      - configuration    = {
          - cpu         = 1 -> null
          - cpu_class   = "normal_cpu" -> null
          - disk_mb     = 10240 -> null
          - price_day   = 7 -> null
          - price_month = 210 -> null
          - ram_mb      = 1024 -> null
          - region      = "ru1" -> null
        } -> null
      - date_create      = "2025-11-19T18:22:58+03:00" -> null
      - description      = "Сервер, созданный с помощью Terraform" -> null
      - hostname         = "test-server" -> null
      - id               = "f03a3dbf-d1d7-4b5e-bdca-0ac66531acd6" -> null
      - image            = {
          - software = {
              - id = 6153 -> null
            } -> null
        } -> null
      - ip_address       = "5.35.90.219" -> null
      - name             = "Test Server" -> null
      - region           = "ru1" -> null
      - slug             = "test-server-164" -> null
      - software_domain  = "" -> null
      - status           = "RUNNING" -> null
      - technical_domain = "" -> null
    }

  # beget_ssh_key.devops will be destroyed
  - resource "beget_ssh_key" "devops" {
      - fingerprint = "00:11:22:33:44:55:66:77:88:99:aa:bb:cc:dd:ee:ff" -> null
      - id          = "27709" -> null
      - name        = "Test terraform key" -> null
      - public_key  = <<-EOT
            ssh-rsa AAAAB...
        EOT -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value:

```
2. Подтвердите принятие изменений вводом ``yes`` в командной строке:
```
  Enter a value: yes

beget_compute_instance.test-server: Destroying... [id=f03a3dbf-d1d7-4b5e-bdca-0ac66531acd6]
beget_compute_instance.test-server: Destruction complete after 1s
beget_ssh_key.devops: Destroying... [id=27674]
beget_ssh_key.devops: Destruction complete after 0s

Destroy complete! Resources: 2 destroyed.
```
