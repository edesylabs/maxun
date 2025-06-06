FROM --platform=$BUILDPLATFORM mcr.microsoft.com/playwright:v1.46.0-noble

# Set working directory
WORKDIR /app

# Install node dependencies
COPY package*.json ./
COPY src ./src
COPY public ./public 
COPY server ./server
COPY tsconfig.json ./
COPY server/tsconfig.json ./server/
# COPY server/start.sh ./  

# Install dependencies
RUN npm install --legacy-peer-deps

# Install Playwright browsers and dependencies
RUN npx playwright install --with-deps chromium

# Create the Chromium data directory with necessary permissions
RUN mkdir -p /tmp/chromium-data-dir && \
    chmod -R 777 /tmp/chromium-data-dir

# Install dependencies
RUN apt-get update && apt-get install -y \
    libgbm1 \
    libnss3 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libdrm2 \
    libxkbcommon0 \
    libglib2.0-0 \
    libdbus-1-3 \
    libx11-xcb1 \
    libxcb1 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxext6 \
    libxi6 \
    libxtst6 \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /tmp/.X11-unix && chmod 1777 /tmp/.X11-unix

# Expose the backend port
EXPOSE ${BACKEND_PORT:-8080}

# Start the backend using the start script
CMD ["npm", "run", "server"]
