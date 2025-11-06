# ters-team.com — Reverse Proxy & Access Engineering

Репозиторий проекта по обеспечению доступности, стабильности и безопасности сайта научной группы **ters-team.com** при доступе из разных регионов (в т.ч. РФ и материковый Китай) с использованием reverse proxy и CDN/edge-подходов.

![Status](https://img.shields.io/badge/status-in%20progress-yellow)
![Docker](https://img.shields.io/badge/docker-ready-blue)
![License: MIT](https://img.shields.io/badge/license-MIT-green)

## Навигация
- [Цели](#цели)
- [Ключевые шаги](#ключевые-шаги-коротко)
- [Структура репозитория](#репозиторий-как-устроен)
- [Быстрый старт](#быстрый-старт-локально)
- [Тонкости reverse proxy](#тонкости-reverse-proxy)
- [Следующие шаги](#следующие-шаги)

## Цели
- Обойти региональные ограничения и повысить доступность ресурса.
- Сохранить корректную работу HTTPS, редиректов, SEO и производительности.
- Подготовить инфраструктуру для переноса на VPS/Cloud и дальнейшей автоматизации (CI/CD, Monitoring).

## Ключевые шаги (коротко)
- Миграция DNS в Cloudflare и тестирование доступности из Китая/РФ.
- Локальный reverse proxy (Docker + nginx) с принудительным IPv4 и корректным SNI.
- Эксперименты с Cloudflare Tunnel (ephemeral → named) и IPv4/http2.
- Зеркало на Netlify (iframe), проверка доступности из РФ, частичная недоступность в Китае.
- Перенос основного домена в Netlify DNS → выявление частичной фильтрации GFW → возврат NS на Wix.
- Текущий фокус: локальный reverse proxy → валидация → перенос на VPS в нейтральном регионе.

Полная хронология: см. `docs/timeline.md`  
Принятые решения: см. `docs/decisions.md`  
Отладка/ошибки: см. `docs/troubleshooting.md`  
Дорожная карта: см. `docs/roadmap_issues.md`

## Репозиторий: как устроен
```
docker/                 # Docker Compose и конфиги nginx
tunnels/                # Cloudflare Tunnel (конфиги/скриншоты)
cloudflare/             # Скриншоты/заметки по Cloudflare DNS
netlify/                # Iframe mirror и Edge Functions (конфиги/скриншоты)
docs/                   # Timeline, Decisions, Troubleshooting, Roadmap
k8s/                    # Плейсхолдеры для Minikube/k3s/managed k8s
terraform/              # Плейсхолдеры для IaC
ci/                     # Плейсхолдер для GitHub Actions
monitoring/             # Плейсхолдеры для Prometheus/Grafana
cloud/                  # Плейсхолдеры для облачных сервисов (AWS,Yandex,Azure)
sre/                    # Плейсхолдеры для отработки SRE-подходов, метрик, SLO/SLA
```

## Быстрый старт (локально)
1. Установить Docker и Docker Compose.
2. Поместить свой финальный `nginx.conf` в `docker/nginx/nginx.conf`.
3. Проверить `docker/docker-compose.yml` (порт публикации, имя сервиса, сеть).
4. Запуск:
   ```bash
   docker compose up -d
   curl -I http://localhost:8080
   ```
   Ожидается `HTTP/1.1 200 OK`.

## Тонкости reverse proxy
- **IPv6**: upstream может резолвиться в AAAA → при отсутствии IPv6-маршрутизации используем `resolver ... ipv6=off;`.
- **SNI**: для TLS к upstream включить `proxy_ssl_server_name on;`.
- **Host-заголовок**: `proxy_set_header Host <upstream_host>;`.
- **sub_filter**: подмена абсолютных ссылок на нужный origin (см. пример в `nginx.conf`).
- **Accept-Encoding**: для корректной подмены HTML/CSS/JS отключаем gzip у upstream: `proxy_set_header Accept-Encoding "";`.

## Следующие шаги
- Развернуть VPS в нейтральном регионе (Сингапур/Турция/Казахстан), перенести reverse proxy.
- Kubernetes (Minikube/k3s → managed): Deployment/Service/Ingress + Helm chart.
- IaC (Terraform), CI/CD (GitHub Actions), Monitoring (Prometheus/Grafana), Cloud, SRE-практики.

## Лицензия
MIT.
