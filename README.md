### USERCASE диаграмма
![USERCASE диаграмма](./USERCASE.jpg)

### ERD диаграмма
![ERD диаграмма](./ERD.png)

    robot {
        id integer pk increments
        model_id integer *> model.id
        mac_adress varchar(12)
        user_group_id integer *>* user_group.id
        user_id integer *> user.id
        status boolean
        servicing datetime
        comment text
        firmware_version varchar(250)
    }
    
    user {
        id integer pk increments
        FIO varchar(100)
        group integer *> user_group.id
        adress varchar(150)
        phone varchar(20)
        email varchar(50)
        password text
    }
    
    user_group {
        id integer pk increments
        name_group integer
    }
    
    work_schedule {
        id integer pk increments
        robot_id integer *> robot.id
        date datetime
        mode integer *> modes.id
    }
    
    modes {
        id integer pk increments
        name varchar(50)
    }
    
    model {
        id integer pk increments
        name varchar(150)
    }

### Пользовательский интерфейс
![пользовательский интерфейс](./UI.jpg)

### UML авторизация пользователя по токен
![пользовательский интерфейс](./UML.jpg)

### OpenAPI

    openapi: 3.0.1
    info:
      title: Робот пылесос
      version: 0.0.1
    servers:
      - url: http://localhost:8080/api/v1
    paths:
        /user:
            get:
                summary: Метод получения списка пользователей
                tags:
                    - Users
                operationId: getAllUsers
                responses:
                    "200":
                        description: Успешный ответ со списком пользователей
                        content:
                            application/json:
                                schema:
                                    $ref: "#/components/schemas/Users"
                    "default":
                        description: Всё остальное
                        content:
                            application/json:
                                schema:
                                    $ref: "#/components/schemas/Error"
            post:
                summary: Метод создания нового пользователя
                tags:
                    - Users
                operationId: createUser
                requestBody:
                    required: true
                    content:
                        application/json:
                            schema:
                                $ref: "#/components/schemas/User"
                responses:
                    "200":
                        description: Успешный ответ добавления нового клиента
                        content:
                            application/json:
                                schema:
                                    $ref: "#/components/schemas/User"
    
    components:
        schemas:
            Robot:
                type: object
                required: 
                  - id
                  - model_id
                  - mac_adress
                  - user_group_id
                  - user_id
                  - status
                  - servicing
                  - comment
                  - firmware_version
                properties: 
                  id:
                    type: integer
                  model_id:
                    type: integer
                    description: id модели робота
                  mac_adress:
                    type: string
                  user_group_id:
                    type: integer
                    description: id группы с доступом к обслуживанию
                  user_id:
                    type: integer
                    description: id пользователя
                  status:
                    type: boolean
                  servicing:
                    type: string
                    format: date-time
                    description: Дата и время обслуживания
                  comment:
                    type: string
                    description: комментарий к заявке на обслуживание
                  firmware_version:
                    type: string
            Model:
                type: object
                required:
                    - id
                    - model_name
                properties: 
                    id:
                        type: integer
                    models_name:
                        type: string
            User:
                type: object
                required:
                    - id
                    - fio
                    - group_id
                    - adress
                    - phone
                    - email
                    - password
                properties: 
                    id:
                        type: integer
                    fio:
                        type: string
                    group_id:
                        type: integer
                        description: id группы к которой пренадлежит пользователь
                    adress:
                        type: string
                    phone:
                        type: string
                        example: +7 (000) 000-00-00
                    email:
                        type: string
                        example: example@mail.ru
                    password: 
                        type: string
            Users:
                type: array
                items:
                    $ref: "#/components/schemas/User"
            Error:
                type: object
                required:
                    - codeError
                    - messageError
                properties:
                    codeError:
                        type: string
                        example: 123f456
                        description: Код ошибки
                    messageError:
                        type: string
                        example: error
                        description: Сообщение ошибки
            UserGroup:
                type: object
                required:
                    - id
                    - name_group
                properties:
                    id:
                        type: integer
                    name_group:
                        type: string
            WorkSchedule:
                type: object
                required:
                    - id
                    - robot_id
                    - date
                    - mode
                properties: 
                    id:
                        type: integer
                    robot_id:
                        type: integer
                        description: id робота
                    date:
                        type: string
                        format: date
                    mode_id:
                        type: integer
            Modes:
                type: object
                required:
                    - id
                    - mode_name
                properties: 
                    id:
                        type: integer
                    mode_name:
                        type: string