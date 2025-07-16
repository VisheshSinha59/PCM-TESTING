# PCM-TESTING
PCM-TESTING is a python-based application for XII th grade science stream(mainly for PCM) students to test to themselves for their Boards Examinations.

To make this code run, you have to makes some changes according to your system configuration. Firstly, you have downlaod the folder 'qbank' and save it into C-Drive users and after that change the directory mapping of the coding with the directory of the folder'qbank' of your system. And, Make it Run. 

import os
import random
from PIL import Image

# ✅ Define subject → chapter → directory mapping
subject_dirs = {
    'maths': {f'chap {i}': rf"c:\users\seema\qbank\ncert_ch_1_topic\Maths\Temp math PYQs\chap {i}"
              for i in range(1, 14)},
    'physics': {f'chap {i}': rf"c:\users\seema\qbank\ncert_ch_1_topic\Physics\Temp physics PYQs\chap {i}"
                for i in range(1, 14)},
    'chemistry': {f'chap {i}': rf"c:\users\seema\qbank\ncert_ch_1_topic\Chemistry\Temp chem PYQs\chap {i}"
                  for i in range(1, 14)},
}

# 1️⃣ Subject selection
subject = input("Select subject (Maths, Physics, Chemistry): ").strip().lower()
if subject not in subject_dirs:
    print(f"Invalid subject. Choose from: {', '.join(subject_dirs.keys())}")
    exit(1)

# 2️⃣ Chapter selection
chap_list = list(subject_dirs[subject].keys())
chap_prompt = "Select chapters (comma-separated) from:\n  " + ", ".join(chap_list) + "\n"
chapter_input = input(chap_prompt).strip().lower()
selected_chapters = [c.strip() for c in chapter_input.split(',')]

# Validate chapters
invalid = [c for c in selected_chapters if c not in subject_dirs[subject]]
if invalid:
    print("Invalid chapters entered:", ", ".join(invalid))
    exit(1)

# 3️⃣ Collect image files
matching_files = []
for chap in selected_chapters:
    d = subject_dirs[subject][chap]
    if not os.path.isdir(d):
        print(f"Warning: folder not found for {chap}: {d}")
        continue
    for fn in os.listdir(d):
        if fn.lower().endswith(('.jpg', '.jpeg', '.png')):
            matching_files.append(os.path.join(d, fn))

# 4️⃣ Check & select images
if len(matching_files) < 10:
    print("Not enough images found.")
    exit(1)

selected_files = random.sample(matching_files, 10)

# 5️⃣ Display them one by one
for img_path in selected_files:
    print(f"Displaying: {os.path.basename(img_path)}")
    Image.open(img_path).show()
    input("Press Enter to view next question...")
