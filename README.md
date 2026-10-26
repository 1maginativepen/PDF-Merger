# PDF Merger

A simple, private, browser-based tool for combining multiple PDF files into one. Built with plain HTML, CSS, and JavaScript. No server, no build step, no Node.js required.

Drop in your PDFs, arrange them in the order you want, and download a single merged document. Your files never leave your device.

## Features

- **Drag and drop or browse** to add multiple PDF files at once
- **Reorder files** by dragging cards, or with the up/down buttons (keyboard friendly)
- **Page count and file size** shown for each file
- **Validation**: unreadable or password-protected PDFs are flagged and skipped automatically
- **100% client-side**: files are processed in your browser and never uploaded anywhere
- **Light and dark mode** that follows your system preference
- **Responsive layout** that works on desktop and mobile
- **Zero dependencies to install**: the only library, pdf-lib, is loaded from a CDN

## Quick Start

1. Download or clone this repository.
2. Open `index.html` in any modern browser (rename `merge-pdfs-standalone.html` to `index.html` if needed).
3. Add at least two PDF files.
4. Arrange them in the order you want.
5. Click **Merge & save** to download `merged-pdfs.pdf`.

That's it. No installation required.

## Tech Stack

| Purpose | Tool |
| --- | --- |
| Structure and styling | HTML5, CSS3 (custom properties, `prefers-color-scheme`) |
| Logic | Vanilla JavaScript (ES6+) |
| PDF merging | [pdf-lib](https://pdf-lib.js.org/) v1.17.1 via cdnjs |
| Typography | Fraunces and IBM Plex Sans via Google Fonts |

## How It Works

1. Selected files are read in the browser with `File.arrayBuffer()`.
2. Each file is loaded with `PDFDocument.load()` to validate it and count its pages.
3. On merge, a new empty `PDFDocument` is created, and the pages of each file are copied in order with `copyPages()`.
4. The result is saved as a `Uint8Array`, wrapped in a `Blob`, and downloaded through a temporary `<a download>` link.

```javascript
const mergedPdf = await PDFDocument.create();

for (const file of files) {
  const pdf = await PDFDocument.load(file.bytes);
  const pages = await mergedPdf.copyPages(pdf, pdf.getPageIndices());
  pages.forEach((page) => mergedPdf.addPage(page));
}

const mergedBytes = await mergedPdf.save();
```

## Project Structure

```
.
├── index.html    # The entire app: markup, styles, and script in one file
└── README.md
```

## Hosting on GitHub Pages

1. Push the repository to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, then select `main` and `/ (root)`.
4. Your tool will be live at `https://<your-username>.github.io/<repo-name>/`.

## Browser Support

Works in current versions of Chrome, Edge, Firefox, and Safari. Requires support for `Blob`, `File.arrayBuffer()`, and CSS `color-mix()`.

## Limitations

- Password-protected PDFs cannot be merged.
- Very large files are limited by your device's available memory.
- An internet connection is needed on first load to fetch pdf-lib and the fonts from their CDNs. To work fully offline, download `pdf-lib.min.js` and reference it locally.

## Privacy

All processing happens locally in your browser. No files, filenames, or usage data are sent to any server.

## License

Released under the [MIT License](https://opensource.org/licenses/MIT). Feel free to use, modify, and share.

## Acknowledgments

- [pdf-lib](https://github.com/Hopding/pdf-lib) by Andrew Dillon for the PDF manipulation engine
