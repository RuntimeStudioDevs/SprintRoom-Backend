# Reglas del Proyecto (RULES.md)

Este documento define las reglas estrictas y normas de convivencia de código para todos los desarrolladores y agentes de Inteligencia Artificial que colaboren en el proyecto SprintRoom-Backend.

## Reglas Generales

1. **PROHIBIDO EL USO DE EMOJIS (100%)**: Queda terminantemente prohibido usar emojis en el código fuente, archivos de configuración, documentación (Markdown), mensajes de commits, PRs o cualquier otro archivo dentro de este repositorio. Sin excepciones.
2. **Idioma**: Todo el código fuente (clases, variables, métodos) debe estar en Inglés. Los comentarios y documentación pueden estar en Español si así se requiere, pero el código debe ser Inglés técnico.
3. **Seguridad**: Nunca versionar contraseñas, secretos, tokens JWT, connection strings de bases de datos productivas o keys de APIs. Estos deben ir siempre en variables de entorno o en archivos locales ignorados (ej. `appsettings.Development.json` o `.env`).

## Reglas de Arquitectura

Consulta el archivo [ARCHITECTURE.md](./ARCHITECTURE.md) para ver las directrices sobre las capas del proyecto (Domain, Application, Infrastructure, Presentation) y los patrones requeridos (Clean Architecture, CQRS, MediatR).
