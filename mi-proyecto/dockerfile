# 1. Seleccionar la imagen base de PHP con Apache
FROM php:8.1-apache

# 2. Actualizar repositorios e instalar dependencias del sistema
RUN apt-get update && apt-get install -y \
    libonig-dev \
    libzip-dev \
    zip \
    unzip \
    git

# 3. Instalar las extensiones PHP necesarias para Laravel: pdo, pdo_mysql, mbstring y zip
RUN docker-php-ext-install pdo pdo_mysql mbstring zip

# 4. Copiar Composer desde la imagen oficial de Composer (opcional, para no instalarlo manualmente)
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 5. Establecer el directorio de trabajo en el contenedor (donde se ejecutará la aplicación)
WORKDIR /var/www/html

# 6. Copiar todo el código del proyecto desde el host al contenedor
COPY . .
# 7. Instalar las dependencias PHP mediante Composer
#    Utiliza --no-dev para omitir paquetes de desarrollo y optimiza el autoloader
RUN composer install --no-dev --optimize-autoloader# 8. Ajustar permisos a la carpeta storage (y bootstrap/cache si es necesario)
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# 9. Exponer el puerto 80 para el servidor web Apache
EXPOSE 80

# 10. (Opcional) Comando para iniciar Apache, aunque ya viene configurado en la imagen base
CMD ["apache2-foreground"]