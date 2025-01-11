# Build Angular app
FROM node:18 AS build
WORKDIR /app

# Install dependencies
COPY package*.json ./

# Installer les dépendances avec les options nécessaires
RUN npm install --legacy-peer-deps

# Build the Angular app
COPY . .
RUN npm run build --prod

# Serve with NGINX
FROM nginx:1.21
WORKDIR /usr/share/nginx/html

# Copier les fichiers construits de l'étape précédente
COPY --from=build /app/dist /usr/share/nginx/html

# Copier le fichier de configuration NGINX
COPY nginx.conf /etc/nginx/nginx.conf

# Exposer le port 80
EXPOSE 80

# Démarrer NGINX
CMD ["nginx", "-g", "daemon off;"]
