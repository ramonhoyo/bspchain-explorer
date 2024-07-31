<h1 align="center">Blockscout</h1>
<p align="center">Explorador de Blockchain para inspeccionar y analizar cadenas EVM.</p>
<div align="center">

[![Blockscout](https://github.com/blockscout/blockscout/workflows/Blockscout/badge.svg?branch=master)](https://github.com/blockscout/blockscout/actions)
[![](https://dcbadge.vercel.app/api/server/blockscout?style=flat)](https://discord.gg/blockscout)

</div>


Blockscout proporciona una interfaz completa y fácil de usar para que los usuarios puedan ver, confirmar e inspeccionar transacciones en blockchains EVM (Ethereum Virtual Machine). Esto incluye Ethereum Mainnet, Ethereum Classic, Optimism, Gnosis Chain y muchas otras **redes de prueba de Ethereum, redes privadas, L2s y cadenas laterales**.

Consulte nuestra [documentación del proyecto](https://docs.blockscout.com/) para obtener información detallada e instrucciones de configuración.

Para preguntas, comentarios y solicitudes de características, consulte la [sección de discusiones](https://github.com/blockscout/blockscout/discussions) o a través de [Discord](https://discord.com/invite/blockscout).

## Acerca de Blockscout

Blockscout permite a los usuarios buscar transacciones, ver cuentas y saldos, verificar e interactuar con contratos inteligentes y ver e interactuar con aplicaciones en la red Ethereum, incluidas muchas bifurcaciones, cadenas laterales, L2s y redes de prueba.

Blockscout es una alternativa de código abierto a los exploradores de bloques centralizados y de código cerrado como Etherscan, Etherchain y otros. A medida que las cadenas laterales y L2s de Ethereum continúan proliferando en entornos tanto privados como públicos, se necesitan herramientas transparentes y de código abierto para analizar y validar todas las transacciones.

## Proyectos Soportados

Blockscout actualmente admite varios cientos de cadenas y rollups en todo el ecosistema blockchain. Ethereum, Cosmos, Polkadot, Avalanche, Near y muchos otros incluyen integraciones con Blockscout. [Una lista completa está disponible aquí](https://docs.blockscout.com/about/projects). Si su proyecto no está en la lista, envíe un PR o [contacte al equipo en Discord](https://discord.com/invite/blockscout).

## Primeros Pasos

Consulte la [documentación del proyecto](https://docs.blockscout.com/) para obtener instrucciones:

- [Requisitos](https://docs.blockscout.com/for-developers/information-and-settings/requirements)
- [Despliegue con Ansible](https://docs.blockscout.com/for-developers/ansible-deployment)
- [Despliegue manual](https://docs.blockscout.com/for-developers/manual-deployment)
- [Variables ENV](https://docs.blockscout.com/for-developers/information-and-settings/env-variables)
- [Opciones de configuración](https://docs.blockscout.com/for-developers/configuration-options)

## Agradecimientos

Nos gustaría agradecer a la [fundación EthPrize](http://ethprize.io/) por su apoyo financiero.

## Contribuir

Consulte [CONTRIBUTING.md](https://www.notion.so/CONTRIBUTING.md) para el protocolo de contribución y solicitud de extracción. Esperamos que los contribuyentes sigan nuestro [código de conducta](https://www.notion.so/CODE_OF_CONDUCT.md) al enviar código o comentarios.

## Licencia

![https://img.shields.io/badge/License-GPL%20v3-blue.svg](https://img.shields.io/badge/License-GPL%20v3-blue.svg)

Este proyecto está licenciado bajo la Licencia Pública General de GNU v3.0. Consulte el archivo [LICENSE](https://www.notion.so/LICENSE) para obtener detalles.

## Despliegue

### **Prerequisitos**

- Docker v20.10+
- Docker-compose 2.x.x+
- Cliente Ethereum JSON RPC en funcionamiento

### Creación de contenedores Docker a partir del código fuente

```bash
git clone git@github.com:BusinessShop/bspchain-explorer.git
cd ./bspchain-explorer/docker-compose
docker compose up --build

```

<aside>
⚠️ Nota: Debe instalar el plugin de Docker Compose. Puede usar docker-compose en su lugar si el plugin compose no está instalado en Docker.

</aside>

[Para más información, pulse aquí.](https://docs.blockscout.com/for-developers/deployment/docker-compose-deployment)

### Servicios

Una vez que se ejecuta Docker Compose, se inician los siguientes servicios:

- backend: Servicio backend del explorador. ([referencia](https://docs.blockscout.com/for-developers/information-and-settings/env-variables/backend-env-variables))
- db: Base de datos PostgreSQL para almacenar datos de bloques, transacciones y direcciones de la blockchain.
- frontend: Servicio frontend del explorador. ([referencia](https://docs.blockscout.com/for-developers/information-and-settings/env-variables/frontend-common-envs))
- nginx: Servicio NGINX que configura internamente los proxy de los demás servicios. ([referencia](https://docs.blockscout.com/for-developers/deployment/frontend-migration/proxy-setup))
- redis: Base de datos Redis.
- sig-provider: Microservicio utilizado por Blockscout para mostrar datos de transacción decodificados y determinar acciones de transacción. ([referencia](https://docs.blockscout.com/for-developers/information-and-settings/env-variables/backend-envs-integrations#sig-provider))
- smart-contract-verifier: Verificador de contratos inteligentes. ([referencia](https://docs.blockscout.com/for-developers/information-and-settings/smart-contract-verification))
- stats: Microservicio para visualizar estadísticas.
- visualizer: Microservicio para visualizar contratos inteligentes.

### Variables de entorno

En la carpeta docker-compose/envs se encuentran los archivos de las variables de entorno a configurar:

- common-blockscout.env: Variables de entorno del servicio backend.
- common-frontend.env: Variables de entorno del servicio frontend.
- common-smart-contract-verifier.env: Variables de entorno del servicio de verificación de smart contracts.
- common-stats.env: Variables de entorno del servicio stats.
- common-visualizer.env: Variables de entorno del servicio visualizer.

<aside>
⚠️ Nota: Existen dos ramas en el repositorio que contienen la mayoría de configuraciones listas para los entornos de mainnet y testnet. Las ramas se llaman master (para mainnet) y testnet (para testnet).

</aside>

### Configuración de common-frontend.env

Las siguientes variables deben configurarse de acuerdo a los dominios o IP asignados para los servicios del backend, stats y visualize.

```bash
NEXT_PUBLIC_API_HOST=localhost/api
NEXT_PUBLIC_API_PROTOCOL=http

NEXT_PUBLIC_STATS_API_HOST=http://localhost:8080
NEXT_PUBLIC_VISUALIZE_API_HOST=http://localhost:8081

```

<aside>
⚠️ Nota: En la infraestructura desplegada en AWS, se utilizaba una instancia de EC2 que usaba el servicio de [Nginx Proxy Manager](https://nginxproxymanager.com/) para configurar los dominios y subdominios de bspscan de manera que apuntaran a las IP y puertos correspondientes. Dependiendo del enfoque que se tome con estas configuraciones, influirá en las variables de entorno previamente descritas.

</aside>

## Indexación

BlockScout puede tardar un tiempo en indexar por completo una cadena. Las cadenas más grandes requieren más tiempo. La indexación comienza desde el principio de la cadena (el bloque actual) y va hacia atrás, hacia el bloque génesis. El bloque génesis es el último bloque indexado durante este proceso. ([referencia](https://docs.blockscout.com/for-developers/indexing))

BlockScout utiliza dos indexadores independientes para indexar el historial de la red y mantenerse al día con los nuevos bloques entrantes. Debido a este proceso, un nodo puede sobrecargarse y no responder a las solicitudes RPC de BlockScout dentro del período de espera designado.

Referencias adicionales:

- [Indexación en Blockscout](https://docs.blockscout.com/for-developers/indexing)
- [¿Cómo soluciono los tiempos de espera del indexador?](https://docs.blockscout.com/for-developers/indexing/how-do-i-fix-indexer-timeouts)

Para más información, consulte la documentación de Blockscout.