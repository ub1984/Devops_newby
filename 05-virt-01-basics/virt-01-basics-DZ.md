Задача 2

Задачи: Выберите один из вариантов платформы в зависимости от задачи.

    1. Высоконагруженная база данных MySql, критичная к отказу;
    2. Различные web-приложения;
    3. Windows-системы для использования бухгалтерским отделом;
    4. Системы, выполняющие высокопроизводительные расчёты на GPU.

Ответ
     1. Физические сервера: Обеспечивают прямой доступ к выделенным ресурсам, что может быть критично для высокой производительности баз данных. Гарантированный доступ к вычислительным ресурсам и управление производительностью.
        Паравиртуализация: Может обеспечить изоляцию ресурсов, управление производительностью и отказоустойчивость. Однако, требует более тщательного планирования и конфигурации для достижения максимальной производительности.

     2. Виртуализация уровня ОС: Обеспечивает легкость масштабирования, управление изоляцией и ресурсами. Где требуется развертывание и масштабирование множества контейнеров.

     3. Паравиртуализация: Позволяет эффективно управлять виртуализированными Windows-системами, обеспечивая изоляцию и управление ресурсами. Легкость миграции и обслуживания.

     4. Физические сервера: Гарантируют прямой доступ к вычислительным ресурсам, включая GPU. Это важно для высокопроизводительных расчетов, требующих максимальной производительности GPU без дополнительного уровня виртуализации.

Задача 3
Сценарии: Выберите один из вариантов платформы в зависимости от задачи.

    1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based-инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
    2. Требуется наиболее производительное бесплатное open source-решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
    3. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows-инфраструктуры.
    4. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.

Ответ
    1. ESXi (оптимизации для Windows и Linux VM)
    2. KVM (Open source, прост в установке и управлении)
    3. Hyper-V Server (Хорошая совместимость с Windows-приложениями)
    4. VirtualBox (Open source, прост в установке и управлении)



Задача 4

Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления
 виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. 
 Если бы у вас был выбор, создавали бы вы гетерогенную среду или нет?
    Ответ:  Нет.           


    Риски: 
    
        Гетерогенные среды могут создавать вызовы в управлении, интеграции и обеспечении безопасности, поэтому они требуют тщательного планирования и управления.
        
        Интеграционные проблемы:
        Отсутствие стандартизированных протоколов может создавать проблемы с взаимодействием между различными виртуализированными средами и требовать разработки доп софта(адаптеров).

        Снижение производительности:
        Использование разных СУВ может привести к дополнительным накладным расходам на уровне производительности из-за необходимости управления разными стеками технологий.

        Сложность управления:
        Различные системы управления виртуализацией (СУВ) могут иметь разные интерфейсы и методы управления, что усложняет процессы администрирования и мониторинга.
        Могут иметь разные API, CLI и методы управления, усложняя автоматизацию и оркестрацию ресурсов.

        Сложности в обучении и поддержке:
        Команда администраторов должна обладать компетенциями в области различных систем виртуализации.
        Необходимость в обучении администраторов по разным технологиям влечет за собой дополнительные расходы на персонал и время.

        Безопасность:
        Гетерогенная среда может быть более подвержена уязвимостям, так как различные платформы требуют различных мер безопасности, что увеличивает сложность конфигурации и обеспечения безопасности в гетерогенной среде.

    Для минимизации этих рисков и проблем.

        Стандартизация:
        При выборе гетерогенной среды использовать стандартизированные протоколы и форматы данных, чтобы упростить интеграцию и обмен информацией.

        Централизованное управление:
        Использование решения для централизованного управления, которые позволяют администрировать различные СУВ из единого интерфейса.

        Автоматизация:
        Внедрение автоматизированных процессов управления и мониторинга снизит сложность обслуживания и минимизирует риски.

        Обучение и сертификация:
        Обучение и сертификация персонала для того, чтобы снизить сложность в обслуживании и улучшить безопасность.