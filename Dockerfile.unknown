# Menggunakan image dasar PHP 8.2 dengan FPM
FROM php:8.2-fpm

# Memperbarui paket dan menginstal dependensi yang diperlukan
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libzip-dev \
    zip \
    unzip \
    git \
    curl \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd zip pdo pdo_mysql exif \
    && docker-php-ext-enable exif

# Menginstal Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Periksa apakah Composer terinstal dengan benar
RUN composer --version

# Mengatur direktori kerja dalam container
WORKDIR /var/www

# Menyalin semua file dari host ke container
COPY . .

# Menjalankan instalasi Composer (tanpa script dan autoloader)
RUN composer install --no-scripts --no-autoloader

# Memberikan izin untuk direktori yang dibutuhkan Laravel
RUN chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache

# Menginstal Node.js versi 16 dan npm
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash - && \
    apt-get install -y nodejs

# Menginstal dependensi Node.js
RUN npm install

# Membuat build produksi (atau gunakan `npm run dev` untuk pengembangan)
RUN npm run build

# Membuka port 9000 untuk PHP-FPM
EXPOSE 9000

# Menjalankan PHP-FPM saat container dijalankan
CMD ["php-fpm"]
