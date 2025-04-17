
import cv2
import pytesseract
import pyperclip
from pathlib import Path

# 📍 Cambia esta ruta si tu Tesseract está en otro sitio
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"

# 🚀 Arrastra aquí tu video y presiona Enter
video_path = input("👉 Arrastra aquí tu video y presiona Enter:\n").strip('"')

# 🎥 Cargar el video
video = cv2.VideoCapture(video_path)
frame_rate = int(video.get(cv2.CAP_PROP_FPS))

frame_count = 0
success = True
texts = []

print("\n⏳ Procesando video y extrayendo texto...\n")

# 🔄 Extrae texto cada 1 segundo
while success:
    success, frame = video.read()
    if not success:
        break
    if frame_count % frame_rate == 0:
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        text = pytesseract.image_to_string(gray, lang='eng')
        if text.strip():
            texts.append(text.strip())
    frame_count += 1

# 📝 Copiar al portapapeles
full_text = "\n\n".join(texts)
pyperclip.copy(full_text)

# 💾 Guardar como archivo .txt junto al video
output_file = Path(video_path).with_suffix('.txt')
with open(output_file, 'w', encoding='utf-8') as f:
    f.write(full_text)

print("\n✅ ¡Texto extraído correctamente!")
print(f"📋 Copiado al portapapeles")
print(f"📄 Guardado en: {output_file}")

