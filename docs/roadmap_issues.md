# Дорожная карта DevOps-инфраструктуры (ters-team.com)

## Milestone: CDN & Networking (GoDaddy + Cloudflare + Netlify) — ✅ выполнено
- [x] Перенос NS на Cloudflare, настройка CNAME/A под Wix.
- [x] Минимальный профиль Cloudflare; аудит TLS/Network.
- [x] Netlify iframe-зеркало `ru.ters-team.com` (ограничения зафиксированы).
- [x] Перенос NS на Netlify DNS → возврат на Wix из-за GFW.

## Milestone: Containerization (Docker, Compose) — ⚙️ в работе
- [x] Контейнер Nginx reverse proxy (финальный `nginx.conf`).
- [x] Compose для локального запуска.
- [ ] Шаблонизация конфигурации (envsubst) и `.env.example`.
- [ ] Проверки/скрипты (`curl` метрики, healthcheck), docs.

## Milestone: Kubernetes Orchestration — 🔜 запланировано
- [ ] Minikube/k3s локальный кластер.
- [ ] Deployment/Service/Ingress, Helm chart.
- [ ] План миграции в managed k8s.

## Milestone: Infrastructure as Code (Terraform) — 🔜 запланировано
- [ ] VPS/VM в нейтральном регионе.
- [ ] VPC/Firewall/DNS.
- [ ] State/модули/best practices.

## Milestone: CI/CD Automation — 🔜 запланировано
- [ ] GitHub Actions: build → test → push image.
- [ ] Deploy на VPS/runner.
- [ ] Нотификации.

## Milestone: Monitoring & Logging — 🔜 запланировано
- [ ] Prometheus/Grafana (nginx/host).
- [ ] Логи: Loki или ELK.
- [ ] Alerting (Alertmanager/Telegram).

## Milestone: Cloud & SRE (AWS, Yandex, GCP, Azure) — 🔜 запланировано
- [ ] Оценка регионов и latency.
- [ ] IAM/Secrets/Backups.
- [ ] SLO/SLA, health-checks, синтетические пробы.
- [ ] Runbooks, post-mortems.
