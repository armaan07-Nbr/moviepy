from moviepy.editor import *
from PIL import Image
import os

# Define paths to the uploaded images
image_paths = [
    "/mnt/data/file-2vfShD6Ea5o6ZMXSjAbL7L.jpg",
    "/mnt/data/file-BRCuRW8EsnHyHbfJdJKHhC.jpg",
    "/mnt/data/file-XRbF9jMYk7JupPShQVzXpg.jpg",
    "/mnt/data/file-Nb6pC6DAJ5PDbctdPuTfG6.jpg",
]

# Resize all images to same dimensions and convert to RGB
resized_images = []
for path in image_paths:
    img = Image.open(path).convert("RGB").resize((720, 1280))
    temp_path = path.replace(".jpg", "_resized.jpg")
    img.save(temp_path)
    resized_images.append(temp_path)

# Define a function to create a simple image clip with text overlay
def create_image_clip(image_path, text, duration=5):
    img_clip = ImageClip(image_path).set_duration(duration)
    txt_clip = TextClip(text, fontsize=50, color='white', font="Arial-Bold", align='center', method='caption',
                        size=(700, None)).set_position(("center", "bottom")).set_duration(duration)
    return CompositeVideoClip([img_clip, txt_clip])

# Create clips for each image with playful captions
texts = [
    "Get ready to party with our little superhero!",
    "Armaan is turning ONE!",
    "Join us on 26th April, 6:00 PM",
    "Sankrayagunta, Chittoor - Don’t miss the fun!",
]
clips = [create_image_clip(img, txt) for img, txt in zip(resized_images, texts)]

# Combine all clips into one video
final_clip = concatenate_videoclips(clips, method="compose")
video_output_path = "/mnt/data/armaan_birthday_invite.mp4"
final_clip.write_videofile(video_output_path, fps=24)
