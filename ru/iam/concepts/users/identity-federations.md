# SAML-совместимые федерации удостоверений

{{ yandex-cloud }} поддерживает идентификацию федераций удостоверений с помощью [SAML 2.0](https://wiki.oasis-open.org/security). Это популярный язык разметки для реализации системы единого входа (Single Sign-On, SSO) — технологии, с помощью которой пользователи могут получать доступ ко множеству приложений без необходимости каждый раз вводить свой логин и пароль. Например, когда вы заходите на сайт и видите кнопку _Войти с помощью Яндекс_, _Google_ или _Facebook_ — все это реализация системы единого входа.

Такой подход получил название _федерация удостоверений_ — когда вся информация об логинах и паролях пользователей хранится у доверенного _поставщика удостоверений_ (Identity Provider, IdP). А поставщик услуг (Service Provider, SP), например {{ yandex-cloud }}, отправляет пользователя проходить аутентификацию на сервере поставщика удостоверений (IdP).

## Для чего нужны федерации в {{ yandex-cloud }}? {#saml-federation-usage}

Большие компании обычно уже имеют настроенную систему управления пользователями и доступом в своей сети, например при помощи Active Directory. В такой компании могут быть тысячи сотрудников и будет тяжело заводить аккаунт на Яндексе для каждого сотрудника и вовремя удалять его, когда сотрудник уволился.

{% include [about-saml-federations](../../../_includes/iam/about-saml-federations.md) %}

Так как процесс аутентификации происходит на стороне сервера IdP, то можно настроить более надежную проверку данных пользователя, например двухфакторную аутентификацию или использование USB-токенов.

## Как происходит аутентификация в федерации {#saml-authentication}

{% include [federated-user-auth](../../../_includes/iam/federated-user-auth.md) %}

Процесс аутентификации показан на диаграмме:

![image](../../../_assets/iam/federations/saml-authentication.svg)

1. Пользователь открывает в браузере ссылку для входа в консоль.
1. Если это первая аутентификация пользователя, то консоль отправляет его на сервер IdP для прохождения аутентификации.

    Если пользователь уже проходил аутентификацию, то информация об этом сохранена в cookie его браузера. Если время жизни cookie не истекло, то консоль управления сразу аутентифицирует пользователя и отправляет на главную страницу. Время жизни cookie указывается при создании федерации.

    Если время жизни cookie истекло, то консоль отправляет пользователя на сервер IdP для повторной аутентификации.
1. Сервер IdP показывает пользователю страницу аутентификации. Например, просит ввести логин и пароль.
1. Пользователь вводит на сервере IdP данные, необходимые для аутентификации.
1. В случае успешной аутентификации сервер IdP отправляет браузер пользователя назад на страницу для входа в консоль управления.
1. Консоль управления спрашивает IAM, добавлен ли такой пользователь в облако. Если да, то консоль управления аутентифицирует пользователя и отправляет на главную страницу.
