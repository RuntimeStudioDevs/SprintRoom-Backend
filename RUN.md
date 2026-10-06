# Guía de Ejecución (RUN.md)

Aquí tienes los comandos rápidos y directos para trabajar con SprintRoom-Backend. Asegúrate de ejecutar estos comandos desde la raíz del proyecto (donde está el archivo .sln).

## Levantar el Proyecto (API)

Para iniciar la aplicación web (API) en modo desarrollo:

```bash
dotnet run --project src/SprintRoom.Api/SprintRoom.Api.csproj
```

Nota: Una vez que arranque, la consola te mostrará las URLs locales (como http://localhost:5000 o similar) donde está escuchando la API. Puedes acceder a Swagger añadiendo /swagger a esa URL.

## Ejecutar las Pruebas

Para correr todas las pruebas automatizadas (Unitarias y de Integración):

```bash
dotnet test
```

## Comandos Adicionales

Si solo necesitas compilar el proyecto para verificar que no hay errores de sintaxis:

```bash
dotnet build
```
