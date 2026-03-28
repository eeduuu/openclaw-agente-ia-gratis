# OpenClaw: Guía de instalación 100% gratis

![OpenClaw](https://img.shields.io/badge/OpenClaw-Agente-blueviolet)
![Gratis](https://img.shields.io/badge/Instalación-Gratis-brightgreen)
![IA](https://img.shields.io/badge/IA-Autónoma-blue)
![Idioma](https://img.shields.io/badge/Idioma-Español-red)

> **Despliegue completo en la nube sin costes de servidor ni de modelos.**

Instálalo en un **servidor privado** para tenerlo activo **24/7** y contrólalo por **Telegram**, todo sin hardware propio y a **coste $0**.

![Guía completa: Cómo instalar OpenClaw totalmente gratis de principio a fin](https://github.com/user-attachments/assets/ac8e00ca-3332-4bfc-a3b5-88a3394640f0)

---

## 📋 Requisitos previos (todo gratis)

Antes de tocar la terminal, asegúrate de tener esto a mano:

1.  **Cuenta AWS (Amazon Web Services):** [Crear cuenta aquí](https://aws.amazon.com/es/free/).
    * *Nota:* Te pedirán tarjeta bancaria para verificar identidad, pero no te cobrarán si sigues esta guía (Capa Gratuita 12 meses).
2.  **Cuenta en GitHub:** Para descargar el código.
3.  **API Key de IA (el cerebro):**
    * Opción recomendada (velocidad/gratis): **[Groq Console](https://console.groq.com/keys)**.
    * Configuración para Qwen 2.5 (vía OpenRouter): **[OpenRouter](https://openrouter.ai/)** (busca modelos "free" como Qwen o usa crédito gratuito).
4.  **Telegram:** Tienes que tener la app instalada.

---

## ☁️ FASE 1: El servidor (AWS)

Vamos a crear el ordenador en la nube donde vivirá tu IA.

1.  Entra en tu consola de AWS y busca **"EC2"**.
2.  Haz clic en el botón naranja **"Lanzar instancia"** (Launch Instance).
3.  **Nombre:** Ponle `Agente-IA-openclaw`.
4.  **Imágenes de aplicaciones y SO:** Selecciona **Ubuntu**.
    * *Importante:* Elige la versión `Ubuntu Server 24.04 LTS (HVM)` o `22.04 LTS` que diga "Apto para la capa gratuita".
5.  **Tipo de instancia:** Selecciona `t2.micro` o `t3.micro` (busca la etiqueta verde "Apto para la capa gratuita").
6.  **Par de claves (Login):**
    * Haz clic en "Crear nuevo par de claves".
    * Ponle un nombre (ej: `clave-agente`).
    * Tipo: `RSA`. Formato: `.pem`.
    * Guarda el archivo descargado en un lugar seguro (aunque usaremos un método más fácil para conectar).
7.  **Configuraciones de red:**
    * Marca las casillas: ☑️ Permitir tráfico SSH, ☑️ Permitir tráfico HTTPS y ☑️ Permitir tráfico HTTP.
8.  **Almacenamiento:** Configura **25 GB** o **30 GB** (límite máximo de la capa gratuita de AWS).
9.  Haz clic en **"Lanzar instancia"**.

---

## 🔌 FASE 2: Conexión y preparación

No necesitas instalar programas complicados en tu PC. Usaremos el navegador.

![Preparación y conexión de OpenClaw mediante el navegador sin descargas](https://github.com/user-attachments/assets/c1dd717f-5ff5-41d0-8d95-8e2343a08e37)

### 1. Conectarse al servidor
1.  En el panel de EC2, ve a "Instancias" y selecciona tu nueva instancia.
2.  Haz clic en el botón **"Conectar"** (arriba a la derecha).
3.  En la pestaña "Conexión de la instancia EC2", deja todo como está y pulsa **"Conectar"**.
4.  Se abrirá una pantalla negra (terminal). ¡Ya estás dentro de tu servidor Linux!

### 2. Instalar Docker (el entorno)
Copia y pega este bloque entero en la terminal y pulsa `Enter`. Esto actualizará el sistema e instalará todo lo necesario de una sola vez:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io docker-compose -y
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

*Nota: Si te pregunta algo en pantalla rosa, pulsa `Enter` para aceptar las opciones por defecto.*

**Importante:** Ahora escribe `exit` y pulsa `Enter` para cerrar la ventana. Vuelve a dar al botón **"Conectar"** de AWS para entrar de nuevo (esto es necesario para aplicar los permisos de usuario).

---

## 🤖 FASE 3: Instalación de OpenClaw

### 1. Descargar el agente
Escribe estos comandos uno a uno:

```bash
git clone https://github.com/Starttoaster/OpenClaw.git
cd OpenClaw
```

### 2. Configurar el "cerebro"
Ahora vamos a editar el archivo de configuración. Sigue estos pasos con cuidado:

1.  Crea el archivo de configuración copiando el ejemplo:
    ```bash
    cp config.example.toml config.toml
    ```
2.  Abre el editor:
    ```bash
    nano config.toml
    ```

**DENTRO DEL EDITOR (NANO):**
Usa las flechas del teclado para bajar. Busca las líneas que configuran el `LLM` (modelo de lenguaje).

* **Si usas Groq (recomendado por velocidad/gratis):**
    Cambia los valores para que queden así:
    ```toml
    [llm]
    provider = "groq"
    model = "llama3-70b-8192" 
    api_key = "TU_API_KEY_DE_GROQ_AQUI"
    ```

* **Configuración para Qwen 2.5 (vía OpenRouter):**
    ```toml
    [llm]
    provider = "openrouter"
    model = "qwen/qwen-2.5-72b-instruct"
    api_key = "TU_API_KEY_DE_OPENROUTER_AQUI"
    ```

> **Comandos rápidos de edición para Nano:**
> 1. Borra el texto antiguo y pega tu API Key.
> 2. Para guardar: Pulsa `Ctrl + O` y luego `Enter`.
> 3. Para salir: Pulsa `Ctrl + X`.

---

## 📱 FASE 4: Conectar con Telegram

Tu agente necesita "cuerpo" en Telegram para hablarte.

![Cómo conectar OpenClaw a Telegram](https://github.com/user-attachments/assets/19a84808-f33c-4564-a8ee-4558d6f4c1d8)

1.  Abre Telegram en tu móvil/PC y busca al usuario **@BotFather**.
2.  Escribe `/newbot`.
3.  Ponle un nombre (ej: `MiAgenteSecreto`).
4.  Ponle un usuario (debe terminar en bot, ej: `Agente007_bot`).
5.  **BotFather te dará un TOKEN** (una cadena larga de letras y números). Cópialo.

**Vuelve a la terminal de AWS:**
1.  Abre de nuevo la configuración: `nano config.toml`
2.  Baja hasta la sección `[telegram]`.
3.  Pega tu token donde dice `bot_token`.
4.  **Paso de Seguridad Obligatorio:** En la sección **allowed_users**, introduce tu ID de Telegram entre corchetes. Ejemplo: allowed_users = [12345678]. Esto evita que personas extrañas consuman tus recursos. Consigue tu ID en el bot **@userinfobot**.
5.  Guarda (`Ctrl+O`, `Enter`) y Sal (`Ctrl+X`).

---

## 🚀 EJECUCIÓN

Ya está todo listo. Ejecuta este comando para encender al agente en segundo plano:

```bash
docker-compose up -d
```

El agente comenzará a descargarse y activarse. Puede tardar 1 o 2 minutos la primera vez.

### ¿Cómo sé si funciona?
Escribe este comando para ver qué está "pensando" el robot:

```bash
docker-compose logs -f
```

Si ves mensajes de colores y texto que dice "Started polling" o similar, ¡felicidades! Ve a tu Telegram, busca a tu bot y dile "Hola".

*(Para salir de la pantalla de logs sin apagar el bot, pulsa `Ctrl + C`)*.

---

## 🛠️ Solución de problemas comunes

* **El bot no contesta en Telegram:**
    * Asegúrate de haber guardado bien el Token en `config.toml`.
    * Verifica los logs con `docker-compose logs -f`. Si dice "Unauthorized", el token está mal.
* **Permisos denegados en Docker:**
    * Recuerda que tras instalar Docker (Fase 2), debiste cerrar la ventana y volver a conectar.
* **Quiero apagarlo:**
    * Escribe `docker-compose down`.

---
*No olvides revisar tu facturación de AWS mensualmente para asegurarte de seguir en la capa gratuita.*

---

## Autor

Esta versión está mantenida por:

💼 [Eduard Pampalona Viladot](https://www.linkedin.com/in/eeduuu-seo-ia/)

<p align="left">
 <a href="https://www.linkedin.com/in/eeduuu-seo-ia/" target="_blank"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn - Eduard Pampalona Viladot" /></a>
</p>

---

## Licencia

Este proyecto mantiene la misma licencia que el repositorio original: **MIT**.  
Consulta el archivo `LICENSE` para ver el texto completo de la licencia.
