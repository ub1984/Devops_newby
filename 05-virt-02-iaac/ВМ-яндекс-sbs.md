Создание пары ключей SSH
https://cloud.yandex.ru/ru/docs/compute/operations/vm-connect/ssh#linux-macos_1
    Linux > ssh-keygen -t ed25519 # в директории ~/.ssh
    Windows > ssh-keygen -t ed25519 # в директории C:\Users\<имя_пользователя>\.ssh\
    macOS > ssh-keygen -t ed25519 # в директории ~/.ssh

Добавление SSH-ключей для других пользователей
    Зайдите на ВМ под именем пользователя, которого вы указали при создании ВМ в консоли управления. Если ВМ создавалась через CLI, то пользователь по умолчанию yc-user.
        Примечание
            Получить информацию о ВМ вместе с пользовательскими метаданными можно с помощью команды:
            yc compute instance get --full <имя_ВМ>    

    Создайте нового пользователя и укажите для него оболочку bash для использования по умолчанию:
    sudo useradd -m -d /home/testuser -s /bin/bash testuser

    Переключитесь на созданного пользователя:
    sudo su - testuser

    Создайте папку .ssh в домашней директории нового пользователя:
    mkdir .ssh

    Создайте в папке .ssh файл authorized_keys:
    touch .ssh/authorized_keys

    Добавьте в файл authorized_keys публичный ключ нового пользователя:
    echo "<публичный_ключ>" > /home/testuser/.ssh/authorized_keys

    Измените права доступа к файлу authorized_keys и каталогу .ssh:
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys

    Отключитесь от ВМ командой exit.
    Перезапустите ВМ.
    Проверьте подключение для нового пользователя:
    ssh testuser@<публичный_IP-адрес_ВМ>

Подключиться к виртуальной машине Windows по RDP
https://cloud.yandex.ru/ru/docs/compute/operations/vm-connect/rdp
    Linux RDP        

    Установите Remmina (бесплатный RDP-клиент для Linux) с помощью команд:
        sudo apt-add-repository ppa:remmina-ppa-team/remmina-next
        sudo apt-get update
        sudo apt-get install remmina remmina-plugin-rdp
        Запустите Remmina.
        Нажмите значок +

    В блоке Profile укажите следующие данные:
        Name: произвольное имя подключения.
        Protocol: RDP - Remote Desktop Protocol.

    На вкладке Basic укажите данные для подключения и авторизации:
        Server: публичный IP-адрес виртуальной машины, к которой необходимо подключиться.
        User Name: Administrator.
        Password.

    Нажмите кнопку Save.

    Подключитесь к удаленной машине двойным нажатием на созданное подключение в списке подключений для быстрого доступа.


    Windows RDP        

        Нажмите Пуск.
        В поле поиска введите Подключение к удаленному рабочему столу и выберите соответствующий пункт.
        В окне Подключение к удаленному рабочему столу в поле Компьютер введите публичный IP-адрес ВМ, к которой необходимо подключиться.
        Нажмите кнопку Подключить.
        Укажите параметры учетной записи:
            Имя пользователя Administrator.
            Пароль.
        Нажмите кнопку ОК.

    Подключиться к виртуальной машине Windows через PowerShell
    https://cloud.yandex.ru/ru/docs/compute/operations/vm-connect/powershell
    
        Создайте объект Credentials, заменив <password> паролем пользователя Administrator, который вы указали при создании ВМ:

            $myUserName = "Administrator"
            $myPlainTextPassword = "<password>"
            $myPassword = $MyPlainTextPassword | ConvertTo-SecureString -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($MyUserName, $myPassword)

        Убедитесь, что имя пользователя и пароль в объекте верны:

            $networkCredential = $credential.GetNetworkCredential()
            $networkCredential | Select-Object UserName, Password

        Создайте переменную для IP-адреса ВМ:

            $ipAddress = "<ip-address>"

        Создайте объект SessionOption. В объекте укажите проверки, которые нужно пропустить:

            $sessionOption = New-PSSessionOption `
            -SkipCACheck `
            -SkipCNCheck `
            -SkipRevocationCheck
        
        Подключитесь к интерактивной сессии:

            $psSession = @{
            ComputerName = $ipAddress
            UseSSL = $true
            Credential = $credential
            SessionOption = $sessionOption
            }
            Enter-PSSession @psSession

        Результат:

            [<ip-address>]: PS C:\Users\$myUserName\Documents>

        Завершите сессию:

            Exit-PSSession

        Создайте сессию для неинтерактивного выполнения команд:

            $session = New-PSSession @psSession

        Просмотрите список открытых сессий:

            Get-PSSession

        Выполните команду на удаленной машине:

            $scriptBlock = { Get-Process }
            $invokeCommand = @{
            ScriptBlock = $scriptBlock
            Session = $session
            }
            Invoke-Command @invokeCommand

        .



Восстановление доступа к виртуальной машине

https://cloud.yandex.ru/ru/docs/compute/operations/vm-connect/recovery-access#cloud-init

    Восстановление доступа к ВМ может понадобиться, если:

    Утрачены учетные данные пользователя ВМ.
    Была изменена публичная часть SSH-ключа.
    Невозможно подключиться по SSH.
    Не загружается ВМ.

