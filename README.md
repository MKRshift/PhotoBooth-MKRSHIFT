# PhotoBooth-MKRSHIFT

```
PhotoBooth-MKRSHIFT/
├── comfy_classes/          # Legacy Python ComfyUI integration (deprecated)
├── gui_classes/            # Legacy Python UI (deprecated)
├── hotspot_classes/        # Hotspot and captive portal integration (legacy)
├── js_app/                 # JavaScript server + ComfyUI client
├── language_file/          # Language translations (norway.json, sami.json, uk.json)
├── workflows/              # ComfyUI workflow definitions (default.json, clay.json, etc.)
├── web_ui/                 # Three.js UI for the photo booth
├── main.py                 # Legacy Python entry point (deprecated)
├── requirements.txt        # Legacy Python dependencies (deprecated)
└── README.md               # Project documentation
```

---

## Documentation

## JavaScript-Only Configuration Guide

PhotoBooth now runs entirely on JavaScript. Prompts live inside ComfyUI workflow JSON files and the UI is powered by three.js.

### Configure styles

- **`workflows/`**: Each workflow JSON contains its own prompt text and style settings. Add new styles by dropping a JSON file in this folder. The JS server exposes the style list at `/api/styles`.

### Run the UI server

```bash
cd js_app
npm install
npm run start
```

The server will be available at `http://localhost:8080` and serves the three.js UI from `web_ui/`.

---

## Installation

Follow these steps to install and set up PhotoBooth.

### 1. Prerequisites

Hardware:

- Camera connected to your computer
- Optional: Raspberry Pi OS (for hotspot features, if using Raspberry Pi)

Software:

- [comfyUI](https://www.comfy.org/) (see below for more details on installing ComfyUI)
- [Node.js](https://nodejs.org/) 18+ for the JS server

---

## ComfyUI runtime notes

When launching ComfyUI, enable preview updates so the UI can display sampling progress:

```bash
python main.py --preview-method taesd
```

If you prefer automatic selection, you can use:

```bash
python main.py --preview-method auto
```

### TTY requirements in containers

Some samplers in ComfyUI expect a TTY for progress output. If you are running inside Docker, make sure you allocate a TTY (for example, `docker run -it ...`) or configure your runtime to attach a pseudo-TTY so progress updates do not stall or disappear.

### Troubleshooting: proxy timeouts

Long-running image generations can exceed reverse-proxy timeouts. If you see 504/timeout errors while a job is still running, increase the proxy timeout values (for example, `proxy_read_timeout`/`proxy_send_timeout` in Nginx or `timeout`/`readTimeout` in your load balancer) to accommodate longer sampling durations.

---

## Legacy Python notes

The previous Python implementation is still checked in for reference, but it is no longer used by the JS workflow. You can remove it after validating the JS build on your target hardware.
