В кодовой базе проекта идет инструмент для генерации ключей, которые могут быть использованы для регистрации пользователя в проекте Голос

После сборки проекта, вы можете найти файл det_dev_key.cpp в директории __/programs/util__ и изучить его.

Для генерации пар ключей, введите в консоли:

```bash
./get_dev_key golosuser active-cryptoonelove owner-cryptoonelove posting-cryptoonelove memo-cryptoonelove othercryptoonelovekey
```
Выводом будет несколько пар ключей:

```bash
[{"private_key":"5JDoZLvTu9xSfH7wiFEg8xPwiWa8gCctdAhz5LTTtKjx9sBBaKU","public_key":"GLS4ybS19Ga2dh3zFJAVhQLTie5dmg4UEVbpK3Vu7ThBqUJeath4j"},
{"private_key":"5JfrHKyz9xh7xwgTJ1AmcSvdGWvGiFhcj16cfsCB6TGtbs3ci9N","public_key":"GLS8XXEaRx855V8jymLgnuAhhvo8WRD4fmUQm9qxsvdq8wxt8Ayu6"},
{"private_key":"5KJ9b7KzrFfkRtmV1miFsR6SYX5Feok9S19VPUoXjBEZJgJz9uJ","public_key":"GLS6cdxbXENrw9G32L3ZcNj8DHDNEF6LU7cuRGzspuc47keyyheng"},
{"private_key":"5J9piaqHugUe2Bhzcn4Ht79sthurBEWViqme1tXnTYCsRSKwZYk","public_key":"GLS7QM8d9whh9VzNEqXynrqb7VgPb1MyZsraDD37Cyzw6pqTFDbtQ"},
{"private_key":"5KN2FYUYb49RDSDpYDvdYJQVBv7xgbyswGUdgKpFqSd79GVkkeh","public_key":"GLS7vpq2yJK3WeNxeFGj4hkPpYzuyxZTpkLHzN1CNLoDyfnFmXiLh"}]
```

```javascript
fc::sha256::hash( dev_key_prefix + arg )
где dev_key_prefix = golosuser
и arg = owner-cryptoonelove | active-cryptoonelove | posting-cryptoonelove | memo-cryptoonelove
```

После вы можете зарегестрировать пользователя, используя __./cli_wallet__:

```javascript
create_account_with_keys creator account_name "json_metadata" owner_public_key active_public_key posting_public_key meme_public_key true
```
Заметка: для этого вы должны задать пароль для __cli_wallet__ и импортировать туда ключи для вашего __creator__ аккаунта, с которого вы будите регистрировать нового участника проекта Голос.

Заметка: в __json_metadata__ вы можете записать метаинформацию для аккаунта. К примеру, все аккаунты, которые будут созданы на генезисе (со снэпшота стима) получат следующую запить: __created_at: GENESIS__. Аккаунты, которую будут инвестировать в проект на стадии ICO, получат записать уникального для каждого инвестора биткоин адреса, на который он сможет перевести инвестиции, которые затем будут автоматически переправлены на мультисиг адрес создателей проекта.
