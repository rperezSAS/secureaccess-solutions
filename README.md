# Despliegue Automatizado de IAM (SSO) con Keycloak

Este repositorio contiene la Infraestructura como Código (IaC) necesaria para desplegar un entorno centralizado de Gestión de Identidades y Accesos (IAM). Utiliza **Ansible** para la automatización del aprovisionamiento y **Docker Compose** para la orquestación de los contenedores. 

El proyecto establece a **Keycloak** como Proveedor de Identidad (IdP), respaldado por PostgreSQL, para suministrar Single Sign-On (SSO) y Autenticación Multifactor (MFA) a aplicaciones cliente (como WordPress) mediante el protocolo **OpenID Connect**.

## 📂 Estructura del Repositorio Explicada

El código está dividido en dos módulos principales: la configuración de automatización (Ansible) y la configuración de contenedores (Docker).

```text
proyectoASIR/
├── README.md                 # Este archivo de documentación técnica.
├── ansible/
│   ├── deploy_keycloak.yml   # Playbook principal de Ansible. Tareas que realiza:
│   │                         # 1. Instala dependencias y el motor de Docker en el servidor destino.
│   │                         # 2. Crea los directorios de trabajo en /opt/.
│   │                         # 3. Transfiere los archivos de la carpeta local 'docker' al servidor.
│   │                         # 4. Ejecuta 'docker compose up -d' remotamente.
│   └── inventario.ini        # Archivo de hosts. Define la IP del servidor objetivo y pasa las variables
│                             # de entorno SSH (usuario y contraseñas) para la elevación de privilegios (become).
└── docker/
    ├── .env                  # (Ignorado en Git) Archivo local con las contraseñas reales.
    ├── .env.example          # Plantilla base. Mapea las variables que consumirá el docker-compose:
    │                         # credenciales de base de datos (POSTGRES_*) y de Keycloak (KEYCLOAK_ADMIN_*).
    └── docker-compose.yml    # Receta de contenedores. Levanta dos servicios aislados en una red bridge:
                              # - 'postgres:15' (Base de datos relacional para Keycloak, con volumen persistente).
                              # - 'keycloak:24.0.0' (Servidor IAM mapeado al puerto 8080).
