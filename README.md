# Cloud Print Server

A cloud-based printing service that lets users upload documents or images from a browser and send print jobs to a local Windows printer connected to a machine running the local agent.

This project is designed for simple cloud printing workflows where the browser acts as the client, the Node.js server acts as the cloud relay, and a Python agent installed on a Windows PC handles the actual printer interaction.

Live demo: https://cloud-print-server-iota.vercel.app

## Overview

The repository contains three main pieces:

- `server.js`: Node.js + Express web server that receives print jobs, tracks registered printers, and exposes APIs for the browser and local agent.
- `agent.py`: Python Windows agent that registers installed printers, polls for pending print jobs, and sends the file to the local printer silently in the background.
- `public/`: Browser UI for uploading files, selecting a printer, previewing the document, and sending print requests.

## Features

- Upload PDFs and images from the browser
- Select a target printer from the connected local machine
- Adjust print settings such as:
  - copies
  - page orientation
  - color mode
  - paper size
- Preview uploaded files before sending them to print
- Set a default printer remotely from the web UI
- Polling-based communication between the client browser and the local Windows agent
- Lightweight deployment using Vercel for the cloud server

## Architecture

The application uses a simple polling model:

1. A Windows machine runs `agent.py`.
2. The agent registers the local machine's installed printers with the cloud server.
3. The browser loads `public/index.html` and fetches the list of available printers from `/api/printers`.
4. The user uploads a file, selects a printer, and submits a print request.
5. The cloud server stores the pending job.
6. The local Windows agent periodically checks for jobs and prints the document to the chosen printer.

## Project Structure

```text
cloud-print-server/
├── agent.py
├── package.json
├── server.js
├── vercel.json
├── public/
│   ├── index.html
│   ├── manifest.json
│   └── server.html
└── README.md
