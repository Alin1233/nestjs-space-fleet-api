# Use Nginx Alpine
FROM nginx:alpine

# Remove default nginx configuration
RUN rm /etc/nginx/conf.d/default.conf

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/conf.d/

# Copy the static website files to the Nginx html directory
COPY index.html /usr/share/nginx/html/index.html

# Copy a simple script to generate env-config.js
# We'll create this script inline or copy a file. Let's do it inline in the CMD for simplicity, 
# or better yet, create a small shell script.
# Creating a shell script to handle startup:
RUN echo '#!/bin/sh' > /docker-entrypoint.sh && \
    echo 'echo "window.env = { API_URL: \"${API_URL:-http://localhost:3000}\" };" > /usr/share/nginx/html/env-config.js' >> /docker-entrypoint.sh && \
    echo 'exec "$@"' >> /docker-entrypoint.sh && \
    chmod +x /docker-entrypoint.sh

# Expose port 80
EXPOSE 8080

# Use the entrypoint script to generate config before starting nginx
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
