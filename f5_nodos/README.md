# Playbook de Actualización de F5 BIG-IP

Este proyecto utiliza Ansible para automatizar el proceso de actualización de software en dispositivos F5 BIG-IP, verificando la versión actual, instalando una nueva imagen, configurando el volumen de arranque y validando el resultado.

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
  - Muestra la versión actual y los volúmenes disponibles.

- **Validación de imagen:**
  - Verifica que la imagen de actualización esté presente en el dispositivo.

- **Instalación de software:**
  - Instala la nueva imagen en el volumen especificado.
  - Configura el volumen de arranque para el próximo reinicio.

- **Gestión de configuración (opcional):**
  - Permite copiar la configuración del volumen activo al nuevo volumen.

- **Verificación final:**
  - Muestra el resultado de la instalación.
  - Obtiene y muestra el estado final del sistema y los volúmenes.

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
└── f5_actualizacion/
    ├── actualizacion_f5.yml   # Playbook principal para actualización de F5
    └── README.md              # Documentación principal del proyecto
```

---

## Variables y Configuración

- **f5_host**: Dirección IP o hostname del F5 (puede venir de variable de entorno)
- **imagen**: Nombre del archivo de imagen de software a instalar (debe estar en `/shared/images/` en el F5)
- **f5_volumen**: Nombre del volumen donde se instalará la imagen (ejemplo: HD1.2)
- **validate_certs**: Validar certificados SSL (por defecto: false)
- **copiar_config**: Si se debe copiar la configuración del volumen activo al nuevo volumen (bool)
- **volumen_actual**: Nombre del volumen actualmente activo (usado si se copia la configuración)

---

## Flujo de Ejecución

1. **Recopilación de información:**
   - Obtiene información del sistema y volúmenes de software del F5.
   - Muestra la versión actual y los volúmenes disponibles.

2. **Verificación de imagen:**
   - Verifica que la imagen especificada existe en `/shared/images/` en el F5.

3. **Instalación de la actualización:**
   - Instala la nueva imagen en el volumen indicado.
   - Configura el volumen de arranque para el próximo reinicio.

4. **Gestión de configuración (opcional):**
   - Si se indica, copia la configuración del volumen activo al nuevo volumen.

5. **Verificación final:**
   - Muestra el resultado de la instalación.
   - Obtiene y muestra el estado final del sistema y los volúmenes.

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
   ansible-playbook actualizacion_f5.yml -e "imagen=BIGIP-15.1.10-0.0.17.iso f5_volumen=HD1.2 copiar_config=true volumen_actual=HD1.1"
   ```

   Puedes ajustar los parámetros según tus necesidades, por ejemplo:
   - `copiar_config=true` para copiar la configuración del volumen activo al nuevo.

3. **Resultados esperados:**
   - El F5 instala la nueva imagen en el volumen especificado.
   - El volumen de arranque se configura correctamente.
   - Se copia la configuración si está habilitado.
   - Se muestra el estado final del sistema y los volúmenes.

---