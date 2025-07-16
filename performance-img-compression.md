# Image Compression & Conversion Guide (WebP)

This guide helps you convert and compress images into .webp format in bulk using simple CLI commands. Optimizing images improves performance and SEO — especially important for frontend-heavy projects like those in Next.js.


## ⚙️ 1. Installation

### ✅ Required Tools:
               
cwebp         -->      Convert images to .webp       -->    brew install webp                
rsvg-convert     -->   Convert .svg → .png (lightweight) --> brew install librsvg            
inkscape      -->      Alternative SVG → PNG converter  --> brew install --cask inkscape    


## 🖼️ 2. Basic Conversion Commands (Single Image)

### ▶️ Convert a single image to WebP

    cwebp -q 50 -lossless path/to/image.jpg -o path/to/image.webp

• -q 50 – sets compression quality to 50 (adjustable from 0–100)
• -lossless – keeps original quality (optional)
• -o – specifies the output file path

### Examples:

    cwebp -q 70 folder/img/photo.jpg -o folder/img/photo.webp
    cwebp -q 30 folder/img/icon.png -o folder/img/icon.webp


## 📁 3. Bulk Conversion Using Loops

### 🔄 Convert JPG/JPEG/PNG to WebP in /folder/img/

    for i in folder/img/*.{jpg,jpeg,png}; do cwebp -q 50 "$i" -o "${i%.*}.webp"; done

• -q 50 → moderate compression
• Saves .webp in the same folder


## 🔧 High Compression (e.g., code snippets PNGs)

    for i in folder/img/*.png; do cwebp -q 20 "$i" -o "${i%.*}.webp"; done

• -q 20 → higher compression, smaller size
• Ideal for screenshots or less quality-critical assets


### 🧼 Lossless Compression

    for i in folder/img/*.{jpg,jpeg,png}; do cwebp -q 50 -lossless "$i" -o "${i%.*}.webp"; done


## 🗺️ 4. Convert SVG to WebP (via PNG)

Since cwebp doesn't support SVG directly, convert .svg → .png → .webp.

### 🧼 Option A: Using rsvg-convert (recommended)

    for i in folder/img/*.svg; do \
      rsvg-convert "$i" -o "${i%.*}.png" && \
      cwebp -q 50 "${i%.*}.png" -o "${i%.*}.webp" && \
      rm "${i%.*}.png"; \
    done

### 🧼 Option B: Using inkscape

    for i in folder/folder-1/img/*.svg; do \
      inkscape "$i" --export-type=png --export-filename="${i%.*}.png" && \
      cwebp -q 50 "${i%.*}.png" -o "${i%.*}.webp" && \
      rm "${i%.*}.png"; \
    done


## 🔁 5. Convert Images Recursively

If you want to convert all images in all subdirectories:

    find folder/ -type f \( -iname "*.jpg" -o -iname "*.jpeg" -o -iname "*.png" \) \
    -exec bash -c 'cwebp -q 50 "$0" -o "${0%.*}.webp"' {} \;


## 💸 6. Optional: Delete original files after conversion

    for i in folder/img/*.png; do \
      cwebp -q 20 "$i" -o "${i%.*}.webp" && rm "$i"; \
    done


## 🙋‍♂️ 7. Skip files if .webp already exists

    for i in folder/img/*.png; do \
      [ -f "${i%.*}.webp" ] || cwebp -q 50 "$i" -o "${i%.*}.webp"; \
    done


### 🙌 Tips

• Keep your original assets in a version-controlled folder (e.g., /raw-assets/) and generate WebP versions from there.
• Don't use -q values too low unless the use-case tolerates visual loss (e.g., screenshots).
• WebP works best for photographic or complex images. For icons/logos, SVG or optimized PNG or SVGs might still be better.


## 📝 Quick Reference

Quality Setting     Use Case                    File Size       Quality
-q 80-100          High-quality photos         Larger          Excellent
-q 50-70           General web images          Medium          Good
-q 20-40           Screenshots/diagrams        Smaller         Acceptable
-lossless          Critical assets             Varies          Perfect

## 🚀 One-liner for common workflow

Convert all images in current directory with moderate compression:

    for img in *.{jpg,jpeg,png}; do [ -f "$img" ] && cwebp -q 60 "$img" -o "${img%.*}.webp"; done
