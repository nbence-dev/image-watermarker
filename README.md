# Image Watermarker

A Python desktop application for adding semi-transparent watermarks to PNG images. Built with Tkinter for the GUI and Pillow for image processing.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Technical Details](#technical-details)
- [Project Structure](#project-structure)
- [Customization](#customization)

## Features

- Load a PNG image via a file browser dialog
- Live preview of the image (original and watermarked) in the app window
- Watermark consisting of a semi-transparent diagonal line pattern and centered text overlay
- Save the watermarked copy to the same folder as the original (`watermarked_<filename>.png`)
- Automatically opens the saved file after saving
- Window auto-centers on launch across all supported platforms

## Requirements

- Python 3.6+
- [Pillow](https://python-pillow.org/) — `pip install pillow`
- Tkinter (bundled with standard Python)
- Arial font available on the system

## Installation

```bash
git clone https://github.com/nbence-dev/image-watermarker.git
cd image-watermarker
pip install -r requirements.txt
python main.py
```

## Usage

1. Click **Add Image** to select a PNG file.
2. Click **Add Watermark** to apply the watermark and preview the result.
3. Click **Save New Image** to write the file — it opens automatically after saving.
4. Click **Close Application** to exit.

Input and output are both PNG (RGBA). The saved file is placed alongside the original with a `watermarked_` prefix.

## Technical Details

### Watermark composition

The watermark is applied using an RGBA overlay composited onto the original image with `Image.alpha_composite`:

- **Pattern**: diagonal lines drawn every 50px across the full image at low opacity (`alpha=30`)
- **Text**: "Watermark" centered using `textbbox` measurements, rendered at `alpha=80` with Arial 80pt
- **Resampling**: LANCZOS filter used for all preview resizes

### Cross-platform file opening

| OS      | Method                              |
| ------- | ----------------------------------- |
| Windows | `os.startfile()`                    |
| macOS   | `subprocess.run(["open", ...])`     |
| Linux   | `subprocess.run(["xdg-open", ...])` |

### GUI layout

Built with Tkinter's grid layout manager. The image preview frame is fixed at 400×300px (`pack_propagate(False)`). The main window is 445×500px and centered on startup using `winfo_screenwidth` / `winfo_screenheight`.

## Project Structure

```
image-watermarker/
├── main.py           # Application entry point — GUI and watermarking logic
├── requirements.txt  # Pillow dependency
├── watermark.ico     # Window icon
└── README.md
```

## Customization

**Change watermark text**

In `add_watermark()`:

```python
draw.text(position, "Your Text Here", fill=watermark_color_text, font=font)
```

**Adjust transparency**

```python
watermark_color_pattern = (255, 255, 255, 30)  # lower alpha = more transparent
watermark_color_text    = (255, 255, 255, 80)
```
