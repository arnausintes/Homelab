# Homelab

Mi laboratorio personal para aprender **sistemas, redes y cloud** con las manos: un servidor Linux en casa, Docker, monitorización y, después, la misma infraestructura en **AWS** gestionada como código.

Todo lo que monto queda documentado aquí: qué hice, **por qué** lo hice así y qué aprendí (incluidos los errores).

## Objetivos

- Aprender con práctica real lo que se hace en un trabajo de cloud / sistemas.
- Preparar la certificación **AWS Solutions Architect – Associate** (objetivo: antes de septiembre de 2027).
- Tener un portfolio que pueda enseñar en una entrevista.

## Hardware

| Equipo | Papel en el lab | Specs |
|---|---|---|
| Portátil HP (Linux) | Servidor 24/7, conectado por cable al router | Intel i3 · 8 GB RAM · 500 GB |
| PC de sobremesa (Windows 11) | Estación de trabajo y laboratorio de máquinas virtuales | Ryzen 7 2700X (8C/16T) · 16 → 32 GB DDR4 |
| iPad / iPhone | Acceso remoto por SSH (Termius) y MFA | — |

Red: fibra 600 Mb (router Orange). El PC está lejos del router y sale por un repetidor de 100 Mb, por eso el servidor va junto al router.

## Hoja de ruta

- [ ] **Fase 0 – Preparar el terreno**
  - [x] Cuenta de AWS asegurada (MFA, usuario IAM, alerta de gasto) → [docs](docs/fase-0/01-aws-seguridad-cuenta.md)
  - [x] Repositorio de documentación
- [ ] **Fase 1 – Primer servidor Linux** (objetivo: antes de Navidad)
  - [ ] Servidor junto al router con IP fija
  - [ ] Acceso SSH con claves desde PC e iPad
  - [ ] Firewall y endurecimiento básico
  - [ ] Docker y primeros servicios
  - [ ] Monitorización
  - [ ] Acceso remoto seguro desde fuera de casa
- [ ] **Fase 2 – Llevarlo a AWS**: VPC, EC2, security groups, IAM, Terraform
- [ ] **Fase 3 – Laboratorio de virtualización**: varias VMs, redes y subredes, router virtual, Kubernetes ligero, CI/CD

## Diario

| Fecha | Qué hice |
|---|---|
| 2026-11-10 | Fase 0: aseguré la cuenta de AWS y creé este repositorio. |
