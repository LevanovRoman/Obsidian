AUTH_ADFS = {  
    "SERVER": "adfs.yourcompany.com",  
    "CLIENT_ID": "your-configured-client-id",  
    "RELYING_PARTY_ID": "your-adfs-RPT-name",  
    # Make sure to read the documentation about the AUDIENCE setting  
    # when you configured the identifier as a URL!   
    "AUDIENCE": "microsoft:identityserver:your-RelyingPartyTrust-identifier",  
    "CA_BUNDLE": "/path/to/ca-bundle.pem",  
    "CLAIM_MAPPING": {"first_name": "given_name",  
                      "last_name": "family_name",  
                      "email": "email"},
    }

## CLIENT_ID[](https://django-auth-adfs.readthedocs.io/en/latest/settings_ref.html#client-id "Постоянная ссылка на этот заголовок")

- **По умолчанию**:
- **Тип**: `dictionary`

**Требуется**
Установите это значение равным значению, которое вы настроили на своем сервере ADFS как `ClientId` при выполнении `Add-AdfsClient` команды.

Вы можете найти это значение, выполнив команду powershell `Get-AdfsClient` на сервере ADFS и приняв `ClientId` значение.

## CLAIM_MAPPING##
ОТОБРАЖЕНИЕ утверждений[](https://django-auth-adfs.readthedocs.io/en/latest/settings_ref.html#claim-mapping "Постоянная ссылка на этот заголовок")

- **По умолчанию**: `None`
- **Тип**: `dictionary`

Словарь сопоставлений утверждений и полей, который будет использоваться для заполнения учетной записи пользователя в Django. Данные пользователя будут устанавливаться в соответствии с этим параметром при каждом входе в систему.

**Ключ** представляет поле модели пользователя (например, `first_name`), а **значение** представляет краткое название утверждения (например, `given_name`).

пример

AUTH_ADFS = {
    "CLAIM_MAPPING": {"first_name":  "given_name",
                       "last_name":  "family_name",
                      "email": "электронная почта"},
}

Словарь также может сопоставлять дополнительные сведения с учетной записью пользователя Django, используя [расширение User model](https://docs.djangoproject.com/en/stable/topics/auth/customizing/#extending-the-existing-user-model). Установите словарь в качестве значения в параметре CLAIM_MAPPING с ключом в качестве имени User model. Вам нужно будет убедиться, что соответствующее поле существует, прежде чем пользователь пройдет аутентификацию. Это можно сделать, создав приемник для сигнала [post_save](https://docs.djangoproject.com/en/4.0/ref/signals/#post-save), который создает связанный экземпляр при создании `User` экземпляра.

пример

'CLAIM_MAPPING': {'first_name': 'given_name',
                  'last_name': 'family_name',
                  'email': 'upn',
                  'userprofile': {
                      'employee_id': 'employeeid'
                  }}

## CA_BUNDLE[](https://django-auth-adfs.readthedocs.io/en/latest/settings_ref.html#ca-bundle "Постоянная ссылка на этот заголовок")

- **По умолчанию**: `True`
- **Тип**: `boolean` или `string`

Значение этого параметра передается при вызове пакета `Requests` при получении токена доступа из ADFS. Это позволяет управлять проверкой сертификата веб-сервера сервера ADFS.

`True` для использования пакета CA по умолчанию из пакета `requests` .

`/path/to/ca-bundle.pem` позволяет указать путь к файлу пакета CA. Если ваш сервер ADFS использует сертификат, подписанный корневым центром сертификации предприятия, вам нужно будет указать путь к этому сертификату здесь.

`False` отключает проверку сертификата.

Ознакомьтесь с документацией [Запросы](http://docs.python-requests.org/en/master/user/advanced/#ssl-cert-verification) для получения более подробной информации.

Предупреждение

Не устанавливайте это значение равным `False` при производственной настройке. Поскольку мы загружаем определенные настройки с сервера ADFS, это может привести к проблеме безопасности. Например, перехват DNS может привести к тому, что злоумышленник внедрит свой собственный сертификат подписи токена доступа.

## RELYING_PARTY_ID[](https://django-auth-adfs.readthedocs.io/en/latest/settings_ref.html#relying-party-id "Постоянная ссылка на этот заголовок")

- **По умолчанию**:
- **Тип**: `string`

**Требуется**

Установите это значение равным `Relying party trust identifier` значению `Relying Party Trust` (2012) или `Web application` (2016), которое вы настроили в ADFS.

Вы можете найти это значение, выполнив команду powershell `Get-AdfsRelyingPartyTrust` (2012) или `Get-AdfsWebApiApplication` (2016) на сервере ADFS и приняв `Identifier` значение.

## AUDIENCE[](https://django-auth-adfs.readthedocs.io/en/latest/settings_ref.html#audience "Permalink to this headline")

- **Default**:
- **Type**: `string` or `list`

**Требуется**

Установите для этого значение `aud` утверждения, которое ваш сервер ADFS отправляет обратно в токене JWT.

Вы можете найти это значение, выполнив команду powershell `Get-AdfsRelyingPartyTrust` на сервере ADFS и приняв значение `Identifier` . Но будьте осторожны, оно не совсем совпадает, если это не URL.

Примеры

|Идентификатор доверия проверяющей стороны|`aud` значение утверждения|
|---|---|
|ваш-RelyingPartyTrust-идентификатор|microsoft: identityserver: ваш-RelyingPartyTrust-идентификатор|
|[https://adfs.yourcompany.com/adfs/services/trust](https://adfs.yourcompany.com/adfs/services/trust)|[https://adfs.yourcompany.com/adfs/services/trust](https://adfs.yourcompany.com/adfs/services/trust)|


