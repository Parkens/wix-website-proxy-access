# Хронология работ (Timeline)

Ниже — сжатая история инженерных шагов по доступности сайта **ters-team.com**.

## 2025-07 — Блокировка в материковом Китае
- Зафиксирована недоступность `https://ters-team.com` из КНР (GreatFire/OONI).
- Скриншот: `docs/screenshots/china_access_block.png`.

## 2025-07/2025-08 — Подключение Cloudflare DNS и минимальный профиль
- Перенос NS на Cloudflare, настройка CNAME/A под Wix.
- Проксирование включено (orange cloud), аудит TLS/Network/Performance.
- Скриншоты: `cloudflare/screenshots/cloudflare_dns_setup.png`.

## 2025-09 — Проблемы у мобильных операторов РФ
- Подозрения на TLS 1.3/QUIC/IPv6/оптимизации Cloudflare.
- Даже при минимальных настройках периодически недоступен.

## 2025-10 — Локальный reverse proxy (Docker + nginx)
- Собран контейнер, `HTTP/1.1 200 OK` на `localhost:8080`.
- В логах Nginx: попытки к IPv6 → `Network unreachable`.
- Решение: `resolver ... ipv6=off;` + `proxy_ssl_server_name on;` + корректные заголовки.
- Скрин: `docker_nginx_local_test.png`.

## 2025-10 — Временный Cloudflare Tunnel (trycloudflare)
- Первые запуски падают из-за QUIC/IPv6 (ошибки таймаутов).
- Фикс: `--protocol http2 --edge-ip-version 4` → стабильное соединение.
- Скрины: `tunnels/screenshots/trycloudflare_tunnel_start.png`, `trycloudflare_tunnel_errors.png`, `trycloudflare_tunnel_fix_ipv4_http2.png`.

## 2025-10 — Именованный туннель с поддоменом
- `cloudflared login` + `tunnel run <name>`, CNAME `proxy.ters-team.com`.
- Переключение на HTTP/2 для стабильности.
- Скрины: `tunnels/screenshots/cloudflare_cert_upload.png`, `named_tunnel_creation.png`, `named_tunnel_cname_dns.png`, `named_tunnel_proxy_run.png`.

## 2025-10 — Зеркало на Netlify (iframe), домен `ru.ters-team.com`
- Подключение домена, выпуск TLS (Let’s Encrypt), проверка из РФ — работает.
- Скрины: `netlify/iframe/screenshots/netlify_domain_connected.png`, `netlify_domain_test_ru.png`, `netlify_ru_domain_russia_check.png`.

## 2025-10 — Перенос основного домена в Netlify DNS → возврат на Wix
- Делегирование NS на Netlify NSONE, wildcard-сертификат.
- По результатам — Netlify частично фильтруется GFW → возврат NS к Wix, домен обратно к Wix.
- Скрины: `netlify/iframe/screenshots/netlify_dns_transfer.png`, `netlify_dns_confirm.png`, `netlify_main_domain_connect.png`, `docs/screenshots/china-netlify-block.png`, `docs/screenshots/website-back-to-wix.png`.

## Текущий статус
- Reverse proxy отрабатывается локально (Docker + Compose).
- Далее — тестирование через туннель и перенос на VPS в нейтральной локации.