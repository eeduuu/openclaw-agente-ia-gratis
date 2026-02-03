# OpenClaw (Privado y Gratis): Tu Agente de IA Autónomo con Qwen y AWS

> **Estado:** 🟢 Activo | **Coste:** 0€ (Usando AWS Free Tier + API Gratuita) | **Modelo:** Qwen / Llama 3

Esta guía te permite instalar **OpenClaw** (anteriormente MoltBot) en tu propio servidor privado. A diferencia de ejecutarlo en tu PC, aquí tendrás un agente autónomo 24/7 que trabaja por ti, gestionado desde Telegram, sin coste de hardware y con total privacidad.

---

## 📋 Requisitos Previos (Todo Gratis)

Antes de tocar la terminal, asegúrate de tener esto a mano:

1.  **Cuenta AWS (Amazon Web Services):** [Crear cuenta aquí](https://aws.amazon.com/es/free/).
    * *Nota:* Te pedirán tarjeta bancaria para verificar identidad, pero no te cobrarán si sigues esta guía (Capa Gratuita 12 meses).
2.  **Cuenta en GitHub:** Para descargar el código.
3.  **API Key de IA (El Cerebro):**
    * Opción Recomendada (Velocidad/Gratis): **[Groq Console](https://console.groq.com/keys)**.
    * Opción Video (Modelo Qwen): **[OpenRouter](https://openrouter.ai/)** (Busca modelos "Free" como Qwen o usa crédito gratuito).
4.  **Telegram:** Tienes que tener la app instalada.

---

## ☁️ FASE 1: El Servidor (AWS)

Vamos a crear el ordenador en la nube donde vivirá tu IA.

1.  Entra en tu consola de AWS y busca **"EC2"**.
2.  Haz clic en el botón naranja **"Lanzar instancia"** (Launch Instance).
3.  **Nombre:** Ponle `Agente-IA-Gratis`.
4.  **Imágenes de aplicaciones y SO:** Selecciona **Ubuntu**.
    * *Importante:* Elige la versión `Ubuntu Server 24.04 LTS (HVM)` o `22.04 LTS` que diga "Apto para la capa gratuita".
5.  **Tipo de instancia:** Selecciona `t2.micro` o `t3.micro` (Busca la etiqueta verde "Apto para la capa gratuita").
6.  **Par de claves (Login):**
    * Haz clic en "Crear nuevo par de claves".
    * Ponle un nombre (ej: `clave-agente`).
    * Tipo: `RSA`. Formato: `.pem`.
    * Guarda el archivo descargado en un lugar seguro (aunque usaremos un método más fácil para conectar).
7.  **Configuraciones de red:**
    * Marca las casillas: ☑️ Permitir tráfico SSH, ☑️ Permitir tráfico HTTPS, ☑️ Permitir tráfico HTTP.
8.  **Almacenamiento:** Puedes subirlo hasta **30 GB** (el máximo gratis). Pon 28 GB para estar seguro.
9.  Haz clic en **"Lanzar instancia"**.

---

## 🔌 FASE 2: Conexión y Preparación

No necesitas instalar programas complicados en tu PC. Usaremos el navegador.

### 1. Conectarse al Servidor
1.  En el panel de EC2, ve a "Instancias" y selecciona tu nueva instancia.
2.  Haz clic en el botón **"Conectar"** (arriba a la derecha).
3.  En la pestaña "Conexión de la instancia EC2", deja todo como está y pulsa **"Conectar"**.
4.  Se abrirá una pantalla negra (Terminal). ¡Ya estás dentro de tu servidor Linux!

### 2. Instalar Docker (El entorno)
Copia y pega este bloque entero en la terminal y pulsa `Enter`. Esto actualizará el sistema e instalará todo lo necesario de una sola vez:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io docker-compose -y
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

*Nota: Si te pregunta algo en pantalla rosa, pulsa `Enter` para aceptar las opciones por defecto.*

**Importante:** Ahora escribe `exit` y pulsa Enter para cerrar la ventana. Vuelve a dar al botón **"Conectar"** de AWS para entrar de nuevo. (Esto es necesario para aplicar los permisos de usuario).

---

## 🤖 FASE 3: Instalación de OpenClaw

### 1. Descargar el Agente
Escribe estos comandos uno a uno:

```bash
git clone [https://github.com/Starttoaster/OpenClaw.git](https://github.com/Starttoaster/OpenClaw.git)
cd OpenClaw
```

### 2. Configurar el "Cerebro"
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
Usa las flechas del teclado para bajar. Busca las líneas que configuran el `LLM` (Modelo de lenguaje).

* **Si usas Groq (Recomendado por velocidad/gratis):**
    Cambia los valores para que queden así:
    ```toml
    [llm]
    provider = "groq"
    model = "llama3-70b-8192" 
    api_key = "TU_API_KEY_DE_GROQ_AQUI"
    ```

* **Si prefieres Qwen (Como en el video, vía OpenRouter):**
    ```toml
    [llm]
    provider = "openrouter"
    model = "qwen/qwen-2.5-72b-instruct"
    api_key = "TU_API_KEY_DE_OPENROUTER_AQUI"
    ```

> **Truco Ninja para Nano:**
> 1. Borra el texto antiguo y pega tu API Key.
> 2. Para guardar: Pulsa `Ctrl + O` y luego `Enter`.
> 3. Para salir: Pulsa `Ctrl + X`.

---

## 📱 FASE 4: Conectar con Telegram

Tu agente necesita "cuerpo" en Telegram para hablarte.

1.  Abre Telegram en tu móvil/PC y busca al usuario **@BotFather**.
2.  Escribe `/newbot`.
3.  Ponle un nombre (ej: `MiAgenteSecreto`).
4.  Ponle un usuario (debe terminar en bot, ej: `Agente007_bot`).
5.  **BotFather te dará un TOKEN** (una cadena larga de letras y números). Cópialo.

**Vuelve a la terminal de AWS:**
1.  Abre de nuevo la configuración: `nano config.toml`
2.  Baja hasta la sección `[telegram]`.
3.  Pega tu token donde dice `bot_token`.
4.  **(Opcional pero recomendado)** En `allowed_users`, pon tu ID de Telegram (puedes saberlo escribiendo al bot @userinfobot en Telegram) para que solo tú puedas usarlo.
5.  Guarda (`Ctrl+O`, `Enter`) y Sal (`Ctrl+X`).

---

## 🚀 EJECUCIÓN: ¡Dale vida!

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
*Este repositorio es una guía educativa basada en OpenClaw. No olvides revisar tu facturación de AWS mensualmente para asegurarte de seguir en la capa gratuita.*
