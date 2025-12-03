# Hilt: The Sword Interface

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Language](https://img.shields.io/badge/language-C23%20%7C%20C%2B%2B17-orange.svg)
![Platform](https://img.shields.io/badge/platform-Linux-lightgrey.svg)

**Hilt** is a lightweight, high-performance CLI Bible reader based on the [CrossWire SWORD Project](https://crosswire.org/sword/index.jsp). 

Built with **Modern C (C23)** and a **C++17 Bridge**, it provides a minimal yet powerful interface for reading scripture directly from your terminal. It is designed for developers and system enthusiasts who prefer speed and efficiency over GUI applications.

---

## 🚀 Features

* **⚡ High Performance:** Native C implementation with minimal overhead.
* **📖 Smart Querying:**
    * **Single Verse:** `John 3:16`
    * **Ranges:** `Rev 7:1-14` (Iterates through verses)
    * **Whole Chapters:** `Rom 8:`
    * **Abbreviations:** Supports standard book abbreviations (e.g., `Exo`, `Mt`).
* **🇰🇷 Locale Support:** Built-in support for Korean abbreviations (e.g., `요` → `John`, `창` → `Genesis`).
* **⚙️ Configurable Defaults:** Set your preferred Bible version permanently (`-s` flag).
* **🎨 Listing Mode:** Visualize verse structures quickly with grid output (`-l` flag).
* **🌈 Clean Output:** Strips OSIS/XML tags and applies ANSI color formatting for readability.

---

## 📦 Prerequisites

Hilt requires the **SWORD library** installed on your system.

### Arch Linux
```bash
sudo pacman -S sword base-devel
```

### Ubuntu / Debian
```bash
sudo apt install libsword-dev build-essential pkg-config
```

---

## 🛠️ Build & Install

You can build Hilt using `Make` or `CMake`.

### Option 1: Using Makefile (Recommended)
```bash
git clone [https://github.com/YOUR_USERNAME/hilt.git](https://github.com/YOUR_USERNAME/hilt.git)
cd hilt

# Build the project
make

# Install to /usr/local/bin (Optional)
sudo make install
```

### Option 2: Using CMake
```bash
mkdir build && cd build
cmake ..
make
```

---

## 💻 Usage

### 1. Configuration (First Run)
Set your preferred default Bible version. This is saved in `~/.hilt/config`.

```bash
# Set Korean Revised Version as default
hilt -s KorRV

# Or set King James Version
hilt -s KJV
```

### 2. Reading Scripture
Once a default is set, you can omit the version name.

```bash
# Read a single verse (uses default version)
hilt John 3:16

# Read a specific version explicitly
hilt KJV Gen 1:1

# Read a range of verses
hilt Rev 7:1-14

# Read an entire chapter
hilt Rom 8:
```

### 3. Utilities
```bash
# List all verse numbers in Psalm 119 (Grid View)
hilt Ps 119 -l
```

---

## 📚 Managing Bible Modules

Hilt does not come with Bible text data. You must install modules using the SWORD Project's package manager (`installmgr`).

```bash
# 1. Initialize and refresh remote sources
sudo installmgr -init
sudo installmgr -r CrossWire

# 2. Install Modules
# English: King James Version
sudo installmgr -ri CrossWire KJV

# Korean: Korean Revised Version (개역한글)
sudo installmgr -ri CrossWire KorRV
```

> **Note:** Use `sudo` to install modules system-wide, or run without sudo to install to `~/.sword` (user-only).

---

## 🏗️ Project Architecture

Hilt bridges the gap between **C (The Client)** and **C++ (The Library)**.

```
hilt/
├── src/
│   ├── main.c        # Entry point, CLI argument parsing (C23)
│   ├── config.c      # Configuration file management (C23)
│   └── bridge.cpp    # C++ Shim interacting with libsword (C++17)
├── include/
│   ├── bridge.h      # C-compatible header for the C++ bridge
│   └── config.h      # Header for config management
├── tests/            # Unit tests ensuring stability
└── doc/              # Documentation and requirements
```

* **Logic:** The core logic resides in `main.c` using standard C23 features.
* **Bridge:** `bridge.cpp` wraps `libsword` classes (`SWMgr`, `SWModule`, `VerseKey`) and exposes them as opaque pointers (`SwordContext`, `SwordIterator`) to C.

---

## 🤝 Contributing

Contributions are welcome!
1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

Distributed under the **MIT License**. See `LICENSE` for more information.
