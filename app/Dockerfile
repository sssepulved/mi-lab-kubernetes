# Usa una imagen base ligera para servir archivos estáticos
FROM nginx:alpine

# Copia tu archivo HTML a la ubicación donde Nginx lo servirá
COPY index.html /usr/share/nginx/html/index.html

# Expone el puerto 80 (el puerto por defecto de Nginx)
EXPOSE 80

# Comando para iniciar Nginx cuando el contenedor se inicie
CMD ["nginx", "-g", "daemon off;"]