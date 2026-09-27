# Bitácora de Compras

Aplicación web (single-page, un solo archivo HTML) para la gestión del proceso de Compras del holding **Sabbag Radiólogos** y **Ciclotron Caribe**.

## Qué hace

- Gestiona 4 tipos de documento: **Solicitudes de compra (SC)**, **Órdenes de compra (OC)**, **Recepciones (RAR)** y **Facturación**.
- Importación masiva desde Excel (una hoja de cabecera + una de detalle por tipo de documento), con enlace automático entre documentos relacionados (SC → OC → RAR → Factura) por los ids de origen del Excel.
- Bitácora y estado por documento y por ítem, con historial de novedades.
- Para Órdenes de Compra: cálculo automático de cantidad/valor recibido y pendiente por ítem, a partir de las RAR relacionadas (nunca editable a mano), y estado de recepción (Sin recepción / Recepción parcial / Recepción total).
- Exportación a Excel de cada listado.
- Multiempresa (Sabbag / Ciclotron) con los mismos tipos de documento.

## Origen de este código

Este proyecto nació como un [Claude Artifact](https://claude.ai) (una app auto-contenida en un solo HTML), construido de forma iterativa por Anderson junto con Claude. Este repositorio es un punto de partida para seguir modificándolo por fuera del entorno de Artifacts.

## Estructura

- `index.html` — la aplicación completa (HTML + CSS + JavaScript, sin dependencias externas de build).

## Cómo correrlo

Al ser un solo archivo HTML autocontenido, basta con abrirlo en un navegador. **Importante:** en su forma actual, el archivo usa APIs específicas del entorno de Claude Artifacts para persistencia de datos (`window.claude.use('db' | 'downloads' | 'user')`). Fuera de ese entorno esas llamadas no existen, así que para usarlo como base de un proyecto independiente hay que:

1. Reemplazar la capa de almacenamiento (`state.db`, funciones `loadColeccion`, `getAllCached`, etc.) por un backend propio (por ejemplo Firebase, Supabase, una API REST propia, o `localStorage`/IndexedDB para una versión 100% cliente).
2. Reemplazar `state.downloadsCap` (exportación de Excel) por una librería de descarga de archivos en el navegador (por ejemplo `file-saver` o simplemente un enlace `<a download>`).
3. Reemplazar `state.userCap` (identidad del usuario) por tu propio sistema de autenticación, si lo necesitas.

El resto de la aplicación (UI, routing, lógica de negocio de compras/recepciones/estados) es JavaScript plano y no depende de Artifacts.
