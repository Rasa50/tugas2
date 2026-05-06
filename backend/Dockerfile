FROM python:3.9-slim
WORKDIR /app
# Copy requirements duluan untuk memanfaatkan cache [cite: 150, 158]
COPY requirements.txt .
# Jalankan install library [cite: 151]
RUN pip install --no-cache-dir -r requirements.txt
# Baru copy seluruh kode aplikasi [cite: 152]
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]