import secrets
import hashlib
import string
import io
import time
import qrcode
import picamera
import pygame
import RPi.GPIO as GPIO

# --- GPIO setup ---
BUTTON_PIN = 17  # Button 1 GPIO pin, adjust for SeedSigner
GPIO.setmode(GPIO.BCM)
GPIO.setup(BUTTON_PIN, GPIO.IN, pull_up_down=GPIO.PUD_UP)

# --- Pygame setup ---
SCREEN_SIZE = 240
pygame.init()
screen = pygame.display.set_mode((SCREEN_SIZE, SCREEN_SIZE))
pygame.display.set_caption("BYOB Passphrase Generator")
font = pygame.font.Font(None, 24)  # Adjust font size as needed

# --- Camera entropy ---
def get_camera_entropy(num_frames=8):
    camera = None
    try:
        camera = picamera.PiCamera()
        camera.resolution = (320, 240)
        camera.framerate = 30
        time.sleep(2)  # Warm-up

        frames = []
        for _ in range(num_frames):
            stream = io.BytesIO()
            camera.capture(stream, format='rgb', use_video_port=True)
            frames.append(stream.getvalue())
            time.sleep(0.02)

        noise = bytearray(frames[0])
        for frame in frames[1:]:
            noise = bytearray(a ^ b for a, b in zip(noise, frame))

        return hashlib.sha256(noise).digest()
    finally:
        if camera:
            camera.close()

# --- Passphrase generation ---
def generate_secure_passphrase(length=24):
    os_entropy = secrets.token_bytes(32)
    camera_entropy = get_camera_entropy()
    combined_seed = hashlib.sha256(os_entropy + camera_entropy).digest()

    alphabet = string.ascii_letters + string.digits
    alphabet_len = len(alphabet)
    seed_int = int.from_bytes(combined_seed, 'big')

    passphrase_chars = []
    for _ in range(length):
        index = seed_int % alphabet_len
        passphrase_chars.append(alphabet[index])
        seed_int //= alphabet_len

    return "".join(passphrase_chars)

# --- Generate QR code in memory ---
def generate_qr_image(passphrase):
    qr = qrcode.QRCode(version=1, box_size=4, border=2)
    qr.add_data(passphrase)
    qr.make(fit=True)
    img = qr.make_image(fill_color="black", back_color="white")
    return pygame.image.fromstring(img.tobytes(), img.size, img.mode).convert()

# --- Display helpers ---
def display_text(lines):
    screen.fill((255, 255, 255))
    y = 20
    for line in lines:
        text_surf = font.render(line, True, (0, 0, 0))
        text_rect = text_surf.get_rect(center=(SCREEN_SIZE//2, y))
        screen.blit(text_surf, text_rect)
        y += 40
    pygame.display.flip()

def display_image(img):
    img = pygame.transform.scale(img, (SCREEN_SIZE, SCREEN_SIZE))
    screen.blit(img, (0, 0))
    pygame.display.flip()

# --- Main flow ---
try:
    # 1. Home screen
    display_text(["BYOB", "PASSPHRASE", "GENERATOR"])
    time.sleep(5)

    # 2. Generating screen
    display_text(["Creating 24-character", "random passphrase..."])
    passphrase = generate_secure_passphrase(length=24)

    # 3. QR code in memory
    qr_img = generate_qr_image(passphrase)
    showing_qr = True
    display_image(qr_img)

    # 4. Event loop: toggle QR ↔ text
    while True:
        pygame.event.pump()  # process internal events
        if GPIO.input(BUTTON_PIN) == 0:  # Button pressed
            time.sleep(0.2)  # Debounce
            showing_qr = not showing_qr
            if showing_qr:
                display_image(qr_img)
            else:
                # Split passphrase into 3 lines of 8 chars
                lines = [passphrase[i:i+8] for i in range(0, 24, 8)]
                display_text(lines)
        time.sleep(0.1)

finally:
    GPIO.cleanup()
    pygame.quit()
