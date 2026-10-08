# Vue Image Gallery

A Vue.js 2 image gallery with lightbox, zoom, and delete functionality.

## Setup Instructions

### Step 1: Install Vue CLI (if not already installed)

Open your terminal/command prompt and run:

```bash
npm install -g @vue/cli
```

### Step 2: Navigate to the project directory

```bash
cd vue-image-gallery
```

### Step 3: Install dependencies

```bash
npm install
```

### Step 4: Add your images

Before running the app, you need to add 15 images to the `public/images/` folder.

Rename your images as:
- Image1.jpeg
- Image2.jpeg
- Image3.jpeg
- Image4.jpeg
- Image5.jpeg
- Image6.jpeg
- Image7.jpeg
- Image8.jpeg
- Image9.jpeg
- Image10.jpeg
- Image11.jpeg
- Image12.jpeg
- Image13.jpeg
- Image14.jpeg
- Image15.jpeg

Place them in: `public/images/`

Note: You can use .jpg, .png, or any other image format. Just make sure to update the filenames in `src/components/ImageGallery.vue` if you use different names or extensions.

### Step 5: Run the application

```bash
npm run serve
```

The app will be available at: http://localhost:8080

## Features

- Responsive image gallery (5 columns on desktop, 3 on tablet, 2 on mobile)
- Click images to open lightbox
- Zoom with mouse wheel (0.5x to 2x)
- Delete images with red button
- Restore last deleted image
- Add title and description to images
- Navigate between images in lightbox

## Troubleshooting

### Images not displaying
- Make sure images are in the `public/images/` folder
- Check that filenames match exactly (case-sensitive)
- Verify file extensions are correct (.jpeg vs .jpg)
- Try opening the image URL directly in browser: http://localhost:8080/images/Image1.jpeg

### npm run serve errors
- Make sure you're in the project directory: `cd vue-image-gallery`
- Run `npm install` to install dependencies
- Clear npm cache: `npm cache clean --force`
- Delete node_modules and package-lock.json, then run `npm install` again

### Vue version problems
- Check package.json to ensure Vue 2 is installed
- If Vue 3 was installed accidentally, run:
  ```bash
  npm uninstall vue
  npm install vue@2.6.14
  ```

### Incorrect image filenames
- Open `src/components/ImageGallery.vue`
- Find the `initializeImages` method
- Update the imageNames array with your actual filenames

### Port 8080 already being used
- The Vue CLI will automatically try the next available port (8081, 8082, etc.)
- Or you can specify a different port:
  ```bash
  npm run serve -- --port 3000
  ```

### Blank page or errors in browser console
- Open browser DevTools (F12)
- Check the Console tab for errors
- Make sure all files are created in the correct locations
- Verify Node.js version (should be 12+ for Vue CLI 5)
