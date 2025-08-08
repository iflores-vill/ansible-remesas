# Playbook de Reinicio y Verificación de F5 BIG-IP

Este proyecto utiliza Ansible para automatizar el proceso de reinicio controlado de un dispositivo F5 BIG-IP, incluyendo la verificación de estado, cambio de volumen de arranque, validación de configuración y creación de respaldos UCS.

---

## Índice

1. [Descripción](#descripción)
2. [Requisitos](#requisitos)
3. [Estructura del Proyecto](#estructura-del-proyecto)
4. [Variables y Configuración](#variables-y-configuración)
5. [Flujo de Ejecución](#flujo-de-ejecución)
6. [Uso](#uso)

---

## Descripción

El playbook realiza las siguientes tareas:

- **Verificación inicial:**
  - Obtiene información del sistema y volúmenes de software del F5.
  - (Opcional) Coloca el sistema en modo offline para mantenimiento.

- **Reinicio y cambio de volumen:**
  - Muestra el estado actual del software y volumen de arranque.
  - Reinicia el F5 arrancando desde el volumen especificado.

- **Verificación post-reinicio:**
  - Espera a que el sistema esté disponible tras el reinicio.
  - Verifica la configuración de virtual servers, pools y VLANs (opcional).
  - Guarda la configuración actual del sistema.

- **Gestión de failover:**
  - Cambia el estado de failover a online (opcional).
  - Muestra el estado de failover.

- **Respaldo y reporte:**
  - (Opcional) Crea un backup UCS post-upgrade.
  - Obtiene y muestra el estado final del sistema.

---

## Requisitos

1. **Herramientas necesarias:**
   - Ansible 2.15 o superior
   - Colección `f5networks.f5_modules`

2. **Variables de entorno requeridas:**
   - `F5_HOST`: IP o hostname del F5
   - `F5_USER`: Usuario administrador del F5
   - `F5_PASSWORD`: Contraseña del usuario F5

---

## Estructura del Proyecto

```bash
ansible_remesas_all/
└── f5_reinicio/
    ├── reinicio_f5.yml   # Playbook principal para reinicio y verificación del F5
    └── README.md         # Documentación principal del proyecto
```

---

## Variables y Configuración

- **f5_host**: Dirección IP o hostname del F5 (puede venir de variable de entorno)
- **f5_volumen**: Nombre del volumen desde el que arrancar tras el reinicio (ejemplo: HD1.2)
- **validate_certs**: Validar certificados SSL (por defecto: false)
- **establecer_offline**: Si se debe poner el F5 en modo offline antes del reinicio (bool)
- **verificar_configuracion**: Si se debe validar la configuración de virtual servers, pools y VLANs tras el reinicio (bool)
- **liberar_offline**: Si se debe devolver el F5 a modo online tras el reinicio (bool)
- **crear_backup**: Si se debe crear un backup UCS post-upgrade (bool)

---

## Flujo de Ejecución

1. **Recopilación de información:**
   - Obtiene información del sistema y volúmenes de software del F5.

2. **Modo offline (opcional):**
   - Ejecuta el comando para poner el F5 en modo offline antes del reinicio.

3. **Verificación de estado:**
   - Muestra el estado actual del volumen de arranque y software instalado.

4. **Reinicio y cambio de volumen:**
   - Reinicia el F5 arrancando desde el volumen especificado.

5. **Espera y validación post-reinicio:**
   - Espera a que el sistema esté disponible nuevamente.
   - (Opcional) Verifica la configuración de virtual servers, pools y VLANs.
   - Guarda la configuración actual.

6. **Gestión de failover:**
   - (Opcional) Cambia el estado de failover a online.
   - Muestra el estado de failover.

7. **Backup y reporte:**
   - (Opcional) Crea un backup UCS post-upgrade.
   - Obtiene y muestra el estado final del sistema.

---

## Uso

1. **Exportar variables de entorno sensibles:**

   ```bash
   export F5_HOST=...
   export F5_USER=...
   export F5_PASSWORD=...
   ```

2. **Ejecutar el playbook:**

   ```bash
   ansible-playbook reinicio_f5.yml -e "f5_volumen=HD1.2 crear_backup=true verificar_configuracion=true"
   ```

   Puedes ajustar los parámetros según tus necesidades, por ejemplo:
   - `establecer_offline=true` para poner el F5 en modo offline antes del reinicio.
   - `liberar_offline=true` para devolverlo a online después.

3. **Resultados esperados:**
   - El F5 se reinicia y arranca desde el volumen especificado.
   - Se valida la configuración de virtual servers, pools y VLANs si está habilitado.
   - Se crea un backup UCS si está habilitado.
   - Se muestra el estado final del sistema y del failover.

---