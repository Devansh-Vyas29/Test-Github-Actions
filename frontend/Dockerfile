FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Create .streamlit directory and config
RUN mkdir -p ~/.streamlit

# Create default Streamlit config if not provided
RUN echo '[theme]\n\
primaryColor = "#FF6B6B"\n\
backgroundColor = "#FFFFFF"\n\
secondaryBackgroundColor = "#F0F2F6"\n\
textColor = "#262730"\n\
font = "sans serif"\n\
\n\
[client]\n\
showErrorDetails = true\n\
\n\
[server]\n\
port = 8501\n\
headless = true\n\
runOnSave = true\n\
maxUploadSize = 200' > ~/.streamlit/config.toml

# Expose port
EXPOSE 8501

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
    CMD curl -f http://localhost:8501/_stcore/health || exit 1

# Run Streamlit
CMD ["streamlit", "run", "streamlit_app.py", "--server.port=8501", "--server.address=0.0.0.0"]