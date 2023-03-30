FROM alpine:3.17 AS base

# Install necessary packages for Nginx and SSL/TLS
RUN apk update && \
    apk upgrade && \
    apk --no-cache add bash nginx openssl && \
    mkdir -p /run/nginx /var/lib/nginx /var/log/nginx /var/tmp/nginx /var/cache/nginx /etc/nginx/conf.d && \
    chown  nginx:nginx /run/nginx /var/lib/nginx /var/log/nginx /var/tmp/nginx /var/cache/nginx /etc/nginx/conf.d /var/run && \
    chmod -R 770 /run/nginx /var/lib/nginx /var/log/nginx /var/tmp/nginx /var/cache/nginx /etc/nginx/conf.d /var/run

# Create a self-signed SSL/TLS certificate
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/nginx-selfsigned.key \
    -out /etc/ssl/certs/nginx-selfsigned.crt \
    -subj "/C=US/ST=State/L=City/O=Organization/OU=Department/CN=example.com"

# Set ownership and permissions for SSL/TLS files
RUN chown -R nginx:nginx /etc/ssl/certs/nginx-selfsigned.crt /etc/ssl/private/nginx-selfsigned.key && \
    chmod 640 /etc/ssl/private/nginx-selfsigned.key && \
    addgroup -S ssl-cert && \
    adduser -D -S -h /var/cache/nginx -s /sbin/nologin -G nginx nginx && \
    addgroup nginx ssl-cert || true

# Copy the custom nginx.conf file
COPY nginx.conf /etc/nginx/nginx.conf

# Use the official Nginx Docker image to run the server
FROM nginx:1.19-alpine

# Copy the SSL/TLS certificate and key from the previous stage
COPY --from=base /etc/ssl/certs/nginx-selfsigned.crt /etc/ssl/certs/
COPY --from=base /etc/ssl/private/nginx-selfsigned.key /etc/ssl/private/

# Copy the custom nginx.conf file from the previous stage
COPY --from=base /etc/nginx/nginx.conf /etc/nginx/nginx.conf

# Re-Create necessary directories and files with correct permissions in layer 2 for nginix user
RUN mkdir -p /run/nginx /var/lib/nginx /var/log/nginx /var/tmp/nginx /var/cache/nginx /etc/nginx/conf.d && \
    chown  nginx:nginx /run/nginx /var/lib/nginx /var/log/nginx /var/tmp/nginx /var/cache/nginx /etc/nginx/conf.d /var/run && \
    chmod -R 770 /run/nginx /var/lib/nginx /var/log/nginx /var/tmp/nginx /var/cache/nginx /etc/nginx/conf.d /var/run

# Switch to the nginx use
USER nginx

# Expose ports 8080 and 443 for HTTP and HTTPS, respectively
EXPOSE 8080 443

# Start Nginx with the custom nginx.conf file and secure settings
CMD ["nginx", "-g", "daemon off;"]