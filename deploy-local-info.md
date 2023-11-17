# DEVELOP IN LOCAL DEVICE WITHOUT DOCKER

- Open all projects in Visual Studio Code (open parent folder in a single vscode)
- Define in application.yml different port for each microservice
- Execute required microservices using Spring Dashboard
* NOTA: si hay errores que no se van borrar las carpetas classes y test-classes de target de todos los proyectos
La causa es que tu usuario de wsl no tiene permisos en esas carpetas creadas como root si has usado docker devcontainers en local
La solución definitiva es:
    1. Poner el usuario en devcontainers.json: "remoteUser": "mmanez"
    2. Poner el usuario en docker-compose.yml: user: mmanez
    3. Si hay librerías de maven en .m2 local que no están en remoto poner en docker-compose.yml, en volumes: - "${HOME}/.m2:/home/mmanez/.m2"
    4. Crear el usuario en Dokerfile
    5. Borrar el contenedor de Docker (si existe)
    6. Reconstruir el container en VSCode

# DEVELOP IN LOCAL DEVICE WITH DOCKER AND DEVCONTAINERS VS CODE EXTENSION

- Each microservice has its own .devcontainer with devcontainer.json, Dockerfile and docker-compose.yml
- Open a VSCODE per microservice
- Reopen in container every microservice
- Execute using Spring Dashboard and access with http://localhost:8083 (with the same port as without docker)
- Internal communication between services is done via http://container-name:808X defined in application-dockerlocal.yml
