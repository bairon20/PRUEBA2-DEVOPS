# Creo la maquina virtual de construccion
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .
# Compila el proyecto generando el directorio /dist
RUN npm run build


# Stage 2: Despliegue seguro en producción
# Usamos la variante oficial de Nginx diseñada nativamente para correr como No-Root
FROM nginxinc/nginx-unprivileged:alpine AS production

# Copia los artefactos estáticos compilados desde la etapa build
COPY --from=build /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/templates/default.conf.template

# Exponer el puerto alto nativo de esta imagen (mayor a 1024)
EXPOSE 8080

# Nota técnica para la defensa: Esta imagen ya se inicializa de forma interna 
# bajo un usuario limitado sin privilegios de administrador ('nginx').
