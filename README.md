### NGINX

<h4 align="center">
  <img alt="NGINX" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/Nginx_logo.svg/440px-Nginx_logo.svg.png">
</h4>

>engine x — по-русски произносится как энджи́нкс или э́нжин-и́кс

## В данном репозитории находится дефолтный конфигурационный файл nginx

# Основные функции:
- Nginx позиционируется производителем как простой, быстрый и надёжный сервер, не перегруженный функциями.
- Применение nginx целесообразно прежде всего для статических веб-сайтов и как обратного прокси-сервера перед динамическими сайтами.

**HTTP-сервер**
<ol>
<li>обслуживание неизменяемых запросов, индексных файлов, автоматическое создание списка файлов, кэш дескрипторов открытых файлов</li>
<li>акселерированное проксирование без кэширования, простое распределение нагрузки и отказоустойчивость</li>
<li>поддержка кеширования при акселерированном проксировании и FastCGI</li>
<li>акселерированная поддержка FastCGI и memcached-серверов, простое распределение нагрузки и отказоустойчивость</li>
<li>модульность, фильтры, в том числе сжатие (gzip), byte-ranges (докачка), chunked-ответы, HTTP-аутентификация, SSI-фильтр</li>
<li>несколько подзапросов на одной странице, обрабатываемые в SSI-фильтре через прокси или FastCGI, выполняются параллельно</li>
<li>поддержка SSL</li>
<li>поддержка PSGI, WSGI</li>
<li>экспериментальная поддержка встроенного Perl</li>
</ol>

**Инициализация массива хеш-ключей**

Функция ngx_hash_keys_array_init просто инициализирует структуру ngx_hash_keys_arrays_t, которая относительно проста и не будет здесь подробно описываться.
Функция ngx_hash_add_key добавляет ключ в массив ключей хеша и дедуплицирует его, который в основном включает в себя следующие 6 процессов:
 - Определите тип ключа (общий ключ / подстановочный знак / подстановочный знак)
```
if (key->len > 1 && key->data[0] == '.') {
    skip = 1;
    goto wildcard;
}

if (key->len > 2) {

    if (key->data[0] == '*' && key->data[1] == '.') {
        skip = 2;
        goto wildcard;
    }

    if (key->data[i - 2] == '.' && key->data[i - 1] == '*') {
        skip = 0;
        last -= 2;
        goto wildcard;
    }
}
```

Переменная skip указывает количество пропущенных строк, последняя переменная указывает конечный индекс строки, тип ключевой строки можно различить по skip.
|   Ключевая строка  | Тип классификации |
| ------------------ |:-----------------:|
| www.example.com    | нет               |
| *.example.com	     | skip=2            |
| .example.com       | skip=1            |
| www.example.*	     | skip=0            |
