# Smart Crop Grid Print Studio

A browser-based photo layout, alignment, and passport-print preparation tool. It provides AI-assisted face alignment, configurable photo cropping, print-sheet layout generation, physical ruler calibration, image management, and print-ready exports — all directly in the browser.

The application uses client-side processing, including facial landmark detection through `@vladmandic/face-api`, so images can be processed without requiring a dedicated backend server.

---

## Features

### 1. AI Smart Crop & Face Alignment

The Smart Crop workflow helps prepare individual photographs for standardized printing and identification-photo layouts.

* Automatic face detection and alignment
* 68-point facial landmark detection
* Automatic eye-level tilt correction
* Manual image positioning and scaling
* Vertical headroom adjustment
* Horizontal face/image centering
* Manual rotation control
* Touch and mouse gesture support
* Parameter lock controls
* Batch crop processing through a crop queue
* Independent Smart Crop settings persistence using `localStorage`

#### Supported Crop Presets

* **35 × 45 mm** — 7:9
* **30 × 35 mm** — 6:7
* **2 × 2 inch** — 1:1
* **3:4**
* Custom aspect ratio
* Custom physical dimensions

Custom dimensions can be specified using:

* Millimeters (`mm`)
* Centimeters (`cm`)
* Inches (`in`)
* Pixels (`px`)

Target PPI can also be configured for custom crop dimensions.

---

### 2. Print Layout & Sheet Grid Engine

Create print-ready photo sheets using configurable paper sizes, orientations, grids, spacing, borders, and backgrounds.

#### Paper Sizes

* A4
* 4 × 6 inch
* 5 × 7 inch

#### Layout Options

* Portrait orientation
* Landscape orientation
* Configurable rows and columns
* Adjustable horizontal and vertical spacing
* Configurable margins
* Multiple measurement units:

  * `mm`
  * `cm`
  * `in`
  * `px`
* PPI presets:

  * 300 PPI
  * 450 PPI
  * 600 PPI
* Locked image aspect ratio
* Configurable background
* Configurable image borders

The layout configuration is persisted locally so that commonly used settings can be retained between sessions.

The sidebar accordion state is also persisted, allowing the interface to reopen with the same sections expanded or collapsed.

---

### 3. Physical Ruler & True 1:1 Screen Calibration

The application includes a physical screen ruler for working with dimensions that need to correspond to real-world measurements.

#### Ruler Features

* Millimeter ruler
* Centimeter ruler
* Inch ruler
* Pixel ruler
* Dynamic scale rendering
* True 1:1 physical-size display
* Screen-density estimation
* Manual calibration
* Screen diagonal-based calibration
* Device-size presets

The calibration system can estimate display characteristics using browser/device information and can also be manually calibrated when greater physical accuracy is required.

#### Device Presets

The interface provides common display-size presets, including:

* POCO X7 Pro — 6.67"
* iPhone — 6.1"
* Laptop — 15.6"
* Monitor — 24"

Manual calibration can be saved or reset as required.

The application calculates physical dimensions using display density and PPI information to provide a closer representation of actual millimeter, centimeter, and inch measurements on the user's screen.

> **Note:** Browser-based physical measurement depends on the actual display, browser scaling, operating-system scaling, and device characteristics. For accurate physical measurement, manual calibration against a known physical reference is recommended.

---

### 4. Image Tray & Image Management

The image tray provides controls for managing imported photographs before generating the final print sheet.

Each image can be individually controlled without removing it from the application.

#### Image Selection

A selection checkbox is available on each thumbnail to:

* Include an image in the print grid
* Exclude an image from the print grid without deleting it

This allows a larger collection of imported images to be maintained while selectively choosing which photographs participate in the current print layout.

#### Thumbnail Actions

Each thumbnail provides quick actions for:

* **Edit** — open the image in the Smart Crop workflow
* **Download** — download the processed image
* **Remove** — remove the image from the tray

The image tray also supports contextual actions through right-click or long-press interaction where available.

---

### 5. Exporting & Downloads

The application provides several export options for prepared photographs and print sheets.

#### Individual Photo Export

Processed photographs can be exported as JPEG images.

The export workflow supports embedding print-resolution information such as DPI metadata.

#### Batch Export

Multiple processed photographs can be downloaded together as a ZIP archive using `JSZip`.

#### Print Sheet Export

The complete photo grid can be rendered as a JPEG print sheet using the configured print dimensions and target PPI.

This makes the generated sheet suitable for further printing or external print workflows.

---

## Technology Stack

The project is implemented as a client-side web application using:

| Technology             | Purpose                                                                 |
| ---------------------- | ----------------------------------------------------------------------- |
| HTML5                  | Application structure and UI                                            |
| CSS3                   | Styling, responsive layout, controls, and visual components             |
| Vanilla JavaScript     | Application logic and state management                                  |
| HTML5 Canvas API       | Image processing, cropping, ruler rendering, and print-sheet generation |
| Tailwind CSS           | Utility-based UI styling                                                |
| `@vladmandic/face-api` | Face detection and facial landmark analysis                             |
| `piexifjs`             | JPEG EXIF/DPI metadata handling                                         |
| `JSZip`                | Batch image ZIP export                                                  |
| `localStorage`         | Persistent application settings                                         |

---

## How It Works

The application follows a browser-based image-processing workflow:

```text
Import Images
     │
     ▼
Image Tray
     │
     ├── Select / Exclude
     │
     └── Edit
          │
          ▼
     Smart Crop
          │
          ├── Face Detection
          ├── Landmark Detection
          ├── Tilt Correction
          ├── Positioning
          ├── Scaling
          └── Custom Crop
          │
          ▼
     Processed Images
          │
          ▼
     Print Layout
          │
          ├── Paper Size
          ├── Orientation
          ├── Rows / Columns
          ├── Spacing
          ├── Margins
          ├── Border
          └── Background
          │
          ▼
     Print Sheet
          │
          ▼
     JPEG Export
```

---

## Client-Side Processing

The application is designed primarily as a client-side tool.

Image operations such as cropping, positioning, canvas rendering, and print-sheet generation are performed in the browser using JavaScript and the HTML5 Canvas API.

Face analysis is also performed through the browser using `@vladmandic/face-api`.

This architecture allows the application to operate without requiring a custom application server for its core image-processing workflow.

---

## Settings Persistence

Application preferences are stored locally using browser `localStorage`.

Persisted settings include relevant:

* Print layout configuration
* Grid configuration
* Crop configuration
* Custom crop settings
* PPI-related preferences
* Physical ruler calibration
* Sidebar accordion state

Smart Crop settings are maintained independently from print-grid settings so that changing the print layout does not unintentionally overwrite crop preferences.

---

## Measurement Units

The application supports multiple measurement systems depending on the selected control.

### Metric

* Millimeter (`mm`)
* Centimeter (`cm`)

### Imperial

* Inch (`in`)

### Digital

* Pixel (`px`)

Physical dimensions are converted using the configured or calculated PPI value.

For example:

```text
pixels = inches × PPI
```

and:

```text
inches = pixels ÷ PPI
```

This allows physical print dimensions and digital canvas dimensions to be coordinated.

---

## PPI Presets

The application provides common print-resolution presets:

| PPI | Typical Use                     |
| --: | ------------------------------- |
| 300 | Standard high-quality printing  |
| 450 | Higher-resolution printing      |
| 600 | High-resolution print workflows |

The selected PPI affects the relationship between physical dimensions and generated pixel dimensions.

---

## Browser Compatibility

The application relies on modern browser APIs, including:

* HTML5 Canvas
* File APIs
* Blob/object URLs
* `localStorage`
* Modern JavaScript
* Browser-based machine-learning inference

A current version of a Chromium-based browser, Firefox, Safari, or another modern browser is recommended.

---

## Project Structure

The project intentionally uses a minimal structure:

```text
project/
├── index.html
└── README.md
```

### `index.html`

The complete web application, including:

* User interface
* Styling
* JavaScript application logic
* Image processing
* Smart Crop workflow
* Print layout engine
* Ruler and calibration functionality
* Image management
* Export functionality

### `README.md`

Project documentation, feature overview, architecture notes, and usage information.

---

## Running the Project

Because the application is primarily client-side, the project does not require a traditional backend application server for its core functionality.

The simplest approach is to serve the project directory using a local HTTP server.

For example, with Python:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

Alternatively, the `index.html` file can be opened directly in a browser where supported. A local HTTP server is generally preferable for consistent browser behavior, especially when external assets or browser security restrictions are involved.

---

## Typical Workflow

### Step 1 — Import Photos

Add the photographs you want to prepare.

### Step 2 — Select Photos

Use the thumbnail selection controls to decide which images should participate in the print grid.

### Step 3 — Smart Crop

Open an image in Smart Crop and select an appropriate preset or define custom dimensions.

Use automatic face alignment or manually adjust:

* Zoom
* Position
* Headroom
* Centering
* Rotation

### Step 4 — Process Images

Process individual images or use the batch crop workflow.

### Step 5 — Configure Print Layout

Choose:

* Paper size
* Orientation
* PPI
* Rows
* Columns
* Spacing
* Margins
* Background
* Borders

### Step 6 — Review the Grid

Check the generated print layout and make any required adjustments.

### Step 7 — Export

Export either:

* Individual processed photographs
* A batch ZIP archive
* The complete print sheet as JPEG

---

## Physical Measurement Calibration

The physical ruler should be calibrated when exact physical sizing is important.

A practical calibration workflow is:

1. Open the physical ruler.
2. Select a suitable device/display preset if available.
3. Compare the on-screen ruler against a known physical measurement.
4. Enter or adjust the calibration information.
5. Save the calibration.
6. Use the ruler at the application's 1:1 physical scale.

Operating-system display scaling, browser zoom, monitor characteristics, and device pixel density can affect physical-size accuracy.

For professional print production, final physical dimensions should always be verified using the actual output rather than relying exclusively on on-screen measurement.

---

## Privacy

The application's core image-processing workflow is browser-based.

Images used by the application are processed through the client-side application rather than requiring a dedicated image-processing backend.

External libraries and CDN-hosted resources may still be loaded by the browser as required by the application.

---

## Limitations

Physical screen measurement cannot be guaranteed to be perfectly accurate without calibration because browser environments do not universally expose the actual physical dimensions of a display.

Face detection and landmark detection can also vary depending on:

* Image quality
* Lighting
* Face orientation
* Occlusion
* Facial visibility
* Browser/device performance

Automatic face alignment should therefore be treated as an assistance mechanism, with manual adjustment available when necessary.

---

## License

No explicit open-source license is currently specified for this project.

Unless a license is added to the project, the source code should not be assumed to be freely reusable, modified, or redistributed.

---

## Author / Project Information

**Smart Crop Grid Print Studio**

A browser-based utility for preparing standardized photographs, arranging them into printable sheets, calibrating physical screen dimensions, and exporting print-ready images.
