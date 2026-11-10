# 01 · Asegurar la cuenta de AWS

**Fecha:** 2026-11-10 · **Fase:** 0

## Objetivo

Dejar la cuenta de AWS como se deja en una empresa antes de empezar a usarla: la cuenta *root* protegida y guardada, un usuario personal para el día a día y una alerta para que el gasto no me sorprenda.

## Qué hice

1. **MFA en la cuenta root** (app de autenticación en el móvil).
2. **Acceso de usuarios IAM a la facturación**: lo activé desde root para no tener que volver a entrar como root para ver costes.
3. **Grupo IAM `Admins`** con la política gestionada `AdministratorAccess`.
4. **Usuario IAM personal** dentro del grupo `Admins`, con acceso a consola.
5. **MFA en el usuario IAM**.
6. **Presupuesto mensual de costes (AWS Budgets)** de 10 $, con aviso por correo al 85 % del gasto real y al 100 % del previsto.

A partir de aquí trabajo siempre con el usuario IAM, no con root.

## Por qué

- **Root tiene poder total** (incluida la facturación y cerrar la cuenta) y no se puede limitar con políticas. Si alguien la roba, lo pierdes todo. Por eso lleva MFA y no se usa en el día a día.
- **Permisos a grupos, no a personas.** Así gestionar el acceso es meter o sacar a alguien de un grupo, no revisar usuario por usuario. Escala a equipos grandes.
- **Todo usuario humano lleva MFA.** Es lo primero que revisa cualquier auditoría de seguridad.
- **En la nube pagas por uso**, y el error más común es dejar recursos encendidos. Una alerta de presupuesto es la primera línea de defensa (disciplina *FinOps*).

## Qué he aprendido

- La diferencia entre la cuenta **root** y los **usuarios IAM**.
- Que por defecto ni un administrador IAM ve la facturación: hay que activarlo desde root.
- Que la consola en español cambia los nombres: *Budgets* = **Presupuestos**, *User groups* = **Grupos de usuarios**.
- El **plan gratuito** actual de AWS: créditos en lugar del antiguo free tier de 12 meses. Dura 6 meses; al terminar, la cuenta se cierra si no pasas al plan de pago.

## Relación con la certificación (SAA-C03)

- Dominio *Design Secure Architectures*: IAM (usuarios, grupos, políticas), MFA, mínimo privilegio, protección de la cuenta root.
- Dominio *Design Cost-Optimized Architectures*: AWS Budgets y control de costes.

## Pendiente / mejoras futuras

- Pasar de `AdministratorAccess` a permisos más ajustados (mínimo privilegio) cuando sepa qué servicios uso.
- Probar **IAM Identity Center**, que es la forma recomendada hoy para gestionar el acceso de personas.
- Decidir antes del fin del plan gratuito si paso al plan de pago.
