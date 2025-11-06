# Troubleshooting

## IPv6 у upstream (Nginx)
**Симптом:** в логах контейнера:
```
connect() failed (101: Network unreachable) upstream: "https://[2606:4700:...]:443"
```
**Причина:** резолв AAAA и отсутствие IPv6-маршрута.
**Фикс (nginx.conf):**
```
resolver 1.1.1.1 8.8.8.8 valid=300s ipv6=off;
proxy_ssl_server_name on;
proxy_set_header Host www.ters-team.com;
proxy_set_header Accept-Encoding "";
sub_filter_once off;
sub_filter 'https://www.ters-team.com' 'http://localhost:8080';
sub_filter_types text/html text/css application/javascript;
```

## trycloudflare: QUIC/UDP/IPv6
**Симптом:** `failed to accept QUIC stream`, `timeout: no recent network activity`.
**Причина:** WSL2/мобильные сети фильтруют UDP/QUIC/IPv6.
**Фикс (cloudflared):**
```
cloudflared tunnel --url http://localhost:8080 --protocol http2 --edge-ip-version 4
```

## Netlify Edge Function: падение при переадресации
**Симптом:** страница "This edge function has crashed", `uncaught exception`.
**Причина:** некорректный rewrite и/или обработка `event.request.url`.
**Шаги:**
- Локальная отладка: `netlify dev --edge-handlers` + `console.log`.
- Проверить парсинг URL/хедеров, валидацию путей, обработку ошибок.

## Netlify частично недоступен из КНР
**Симптом:** GreatFire/OONI показывают `Connection Reset`, `DNS poisoning`.
**Причина:** GFW фильтрует CDN Netlify.
**Решение:** вернуться на Wix DNS/хостинг и/или перенести proxy на VPS в нейтральной зоне.

## Проверочные команды
```
# HTTP заголовки/время:
curl -I -L https://ters-team.com
curl -w "time_connect: %{time_connect}\nstarttransfer: %{time_starttransfer}\n" -o /dev/null -s https://ru.ters-team.com

# DNS/NS:
nslookup -type=ns ters-team.com
nslookup ru.ters-team.com

# Логи контейнера:
docker compose logs -f ters-proxy
```
