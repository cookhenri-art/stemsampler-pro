# 🎵 StemSampler Pro

> **AI-powered stem separation meets advanced polyphonic sampling.**
> A professional VST3/CLAP plugin built in Rust, featuring on-the-fly audio source separation and deep sound design tools.

![Rust](https://img.shields.io/badge/rust-%23000000.svg?style=for-the-badge&logo=rust&logoColor=white)
![VST3](https://img.shields.io/badge/VST3-FF6B35?style=for-the-badge)
![CLAP](https://img.shields.io/badge/CLAP-1DB954?style=for-the-badge)
![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![License](https://img.shields.io/badge/license-Proprietary-red?style=for-the-badge)

---

## ✨ Overview

**StemSampler Pro** loads any audio file, separates it into **4 stems** (Bass, Drums, Vocals, Other) using the HTDemucs AI model, and provides a **full-featured polyphonic sampler** with MIDI keyboard mapping across 128 zones. Each zone has independent loop points, envelopes, LFOs, effects, EQ, and compressor.

Built from the ground up in **Rust** with `nih-plug` and `egui`, optimized for low CPU usage and professional audio quality.

---

## 🖼️ Screenshots

### STEMS Tab — Waveforms, Keyboard, Loop Points
![STEMS Tab](screenshots/01-stems-tab.png)

### ENVELOPPE Tab — 3 Envelopes (Amplitude, Pitch, Filter)
![ENVELOPPE Tab](screenshots/02-envelope-tab.png)

### MOD Tab — 2 Mod Envelopes + 4 LFOs with Cross-Modulation
![MOD Tab](screenshots/03-mod-tab.png)

### MIX Tab — Per-Stem Routing + Master Compressor/Limiter
![MIX Tab](screenshots/04-mix-tab.png)

---

## 🎛️ Features

### 🤖 AI Stem Separation

- **HTDemucs** ONNX model for state-of-the-art source separation
- Automatic GPU detection: **CUDA → DirectML → CPU** fallback
- ~5-10s per 4-minute track on NVIDIA GPU
- Animated progress bar, non-blocking background thread
- Export individual stems to 32-bit WAV
- Upload custom stems with automatic resampling

### 🎹 Polyphonic Sampler

- **128 MIDI zones**, organized by octave:
  - C1-B1 = Bass • C2-B2 = Drums • C3-B3 = Vocals • C4-B4 = Other
- **16-voice polyphony** with intelligent voice stealing
- Draggable loop points (Start/End) on the waveform
- **Velocity** and **polyphonic aftertouch** support
- One-shot or Loop mode • Latch mode
- Right-click context menu: copy/paste/reset/presets/slices

### 🎚️ Sound Shaping

- **3 main envelopes**: Amplitude (pink), Pitch (blue), Filter (orange) — all draggable
- **6 filter types**: LPF, HPF, BPF, Notch, Moog 4-pole, Oberheim
- **2 modulation envelopes** (ENV MOD 1 & 2) — ADSR + amount + destination
- **4 LFOs** with 7 waveforms (Sin, Tri, Saw↑, Saw↓, Sqr, S&H, Random)
- LFO trigger modes: **Free / Retrig / Sync** (DAW-locked)
- **18 modulation destinations** + cross-modulation between LFOs/envelopes

### 🎼 Tempo & Sync

- Automatic BPM detection with multi-section consensus (IOI histogram + comb filter)
- Manual BPM correction: **-1 / +1 / ×2 / ÷2** buttons
- **16 sync divisions**: Off, 1/32, 1/16, 1/8, 1/4, 1/2, 1B, 2B, 4B + **T** (ternary) + **D** (dotted)
- **Phase Vocoder time-stretch** (pitch-preserved) or **RePitch** (vinyl-style)
- Per-stem BPM tracking, DAW transport sync

### 🔊 Effects

Per-zone effects with send routing:

- **Delay** — 3 modes (Repeat / Tail / PingPong), tempo-synced, independent LP filter
- **Reverb** — 5 algorithms (Room, Plate, Hall, Cathedral, Spring) with HPF/LPF
- **Distortion** — 8 types (Soft, Hard, Bitcrush, Fold, Tape, Tube, Rectify, Transistor) + Tone knob
- **Phaser** — 6-stage allpass, Rate + Depth
- **Vinyl** — Noise + Wobble + Bit reduction
- **Stutter** — Tempo-synced glitch effect
- **EQ 4-band** — Graphical parametric (±48dB), interactive drag (L-click freq/gain, R-click Q, scroll Q)
- **Compressor** — Threshold, Ratio, Attack, Release, Makeup + GR meter

### 🎚️ Mix Tab

- **4 independent stem strips** with:
  - Real-time VU meter
  - Volume + Pan
  - Delay + Reverb sends (per stem!)
  - Routing: **Dly→Rev / Rev→Dly / Parallel**
  - Mute / Solo
- **Master Compressor** (Thresh / Ratio / Atk / Rel / Makeup) + output VU + GR meter
- **Master Limiter** (brick wall, ceiling -6 to 0 dB)
- **Per-stem FX buses** — No more effect tails cutting between stems!

### 🎛️ DAW Automation

- **28 automatable parameters per key** × 48 keys = **1344 parameters**
- 7 global parameters (volumes, dry, delay/reverb master vol)
- Full bidirectional sync (UI ↔ DAW, Configure, MIDI Learn)

### 💾 Presets & Persistence

- **Per-key presets** (`.ssp`) and **global presets** (`.ssg`)
- Quick access list (last 15 presets) in context menu
- Full DAW project state persistence
- Undo (Ctrl+Z)

---

## 📦 Installation

### 1. Prerequisites

| | Minimum | Recommended |
|---|---|---|
| **OS** | Windows 10 64-bit | Windows 11 64-bit |
| **RAM** | 4 GB | 8 GB+ |
| **CPU** | Intel i5 / Ryzen 5 | Intel i7 / Ryzen 7 |
| **GPU** | Integrated (CPU fallback) | NVIDIA GTX 1060+ (CUDA) |
| **DAW** | Any VST3 host | Ableton Live, FL Studio, Bitwig, Reaper |
| **Disk** | 200 MB | 200 MB |

### 2. Install Rust

Download and run: https://rustup.rs/
Choose **"1) Proceed with standard installation"** and restart your terminal.

Verify:
```cmd
rustc --version
cargo --version
```

### 3. Install Visual Studio Build Tools

If you already have Visual Studio 2022, you're good.
Otherwise: https://visualstudio.microsoft.com/visual-cpp-build-tools/
→ Check **"Desktop development with C++"**

### 4. Clone and Build

```cmd
git clone https://github.com/yourusername/stem_sampler.git
cd stem_sampler
cargo xtask bundle stem_sampler --release --features onnx
```

The plugin will be created in:
```
target\bundled\stem_sampler.vst3\
```

### 5. Install the Plugin

Copy the folder to:
```
C:\Program Files\Common Files\VST3\
```
*(Administrator rights required)*

Or use the automated script:
```cmd
build_and_package.bat
```

Then rescan plugins in your DAW.

---

## 🎚️ Quick Start

1. Open your DAW, add **StemSampler Pro** as an instrument
2. Click **"Load File"** and pick any WAV, MP3, FLAC, or OGG
3. Wait a few seconds — the AI separation progress bar will run
4. Play notes via MIDI keyboard or piano roll:
   - **C1** = Bass start, **C2** = Drums start, **C3** = Vocals start, **C4** = Other
5. Right-click any key to open the full zone editor menu
6. Explore the tabs: **STEMS** (waveforms), **ENVELOPPE** (ADSR), **MOD** (LFOs), **MIX** (routing)

---

## 🧠 Why Rust?

Compared to traditional C++/JUCE plugin development:

- ✅ **No MSVC toolset headaches** (no constructor issues, no warnings-as-errors)
- ✅ **No `registerBasicFormats()` crash on DLL load**
- ✅ **nih-plug handles VST3 bus layouts correctly by default**
- ✅ **Memory safety** — no segfaults, no dangling pointers
- ✅ **Fearless concurrency** — lock-free atomics for audio ↔ UI thread safety
- ✅ **Simple build**: `cargo xtask bundle` — no CMake, no Projucer
- ✅ **Single-file architecture possible** (our `lib.rs` contains the entire plugin)
- ✅ **Excellent performance** — Rust compiles to native code with LLVM optimizations

---

## 🏗️ Architecture

```
stem_sampler/
├── Cargo.toml              # Dependencies
├── .cargo/config.toml      # Cargo aliases
├── xtask/                  # VST3 bundling tool
│   ├── Cargo.toml
│   └── src/main.rs
├── src/
│   └── lib.rs              # ALL plugin code (~7500 lines)
├── onnxruntime.dll         # AI inference runtime
├── DirectML.dll            # GPU acceleration
├── build_and_package.bat   # Automated build+install
└── README.md
```

---

## 📚 Dependencies

| Crate | Role |
|-------|------|
| **nih_plug** | VST3/CLAP plugin framework |
| **nih_plug_egui** | egui-based UI integration |
| **rustfft** | Phase vocoder FFT |
| **symphonia** | Multi-format audio decoding (WAV/MP3/FLAC/OGG) |
| **parking_lot** | Fast, non-poisoning locks |
| **atomic_float** | Lock-free f32 atomics |
| **rfd** | Native file dialogs |
| **onnxruntime** | AI model inference (HTDemucs) |

---

## 🎨 UI Design

- **Resolution**: 1800×1000 pixels
- **Theme**: Dark with neon accents (pink, blue, orange, green)
- **Layout**: 2-column adaptive (left content + right always-visible EQ/FX)
- **Custom knob widget**: Elektron-style with right-click reset, log/linear scaling
- **Interactive envelope curves**: Drag to shape attack/decay/sustain/release
- **Real-time VU meters**: RMS levels per stem + output peak + gain reduction

---

## 🔬 Technical Highlights

- **Per-stem FX buses** — 4 independent delay + 4 reverb buses (no bleed between stems)
- **CPU optimizations**:
  - Pre-computed compressor/limiter coefficients per buffer
  - Cached atomic loads per buffer
  - Skipped inactive FX buses
  - Fast approximations: `fast_tanh`, `fast_log10`, `fast_db_to_lin`
  - 30 FPS UI repaint throttling
- **BPM detection** — Hybrid IOI histogram + comb filter + multi-section consensus, achieves >95% accuracy on electronic music
- **Phase Vocoder** — FFT 2048, hop 512, Hann window, phase accumulation, overlap-add with RMS normalization
- **ONNX execution provider chain** with heterogeneous error handling via IIFE

---

## 🎯 Roadmap

- [ ] Linux support (via JACK)
- [ ] macOS support (VST3 + AU)
- [ ] Per-zone automation via MPE
- [ ] More AI models (Spleeter, Open-Unmix as alternatives)
- [ ] Spectral filter / Resynthesis mode
- [ ] Granular playback mode

---

## 🤝 Contributing

Contributions are welcome! This is a hobby project built with passion for audio DSP and Rust.

If you find a bug or have a feature request, open an issue.
If you want to contribute code, open a pull request with a clear description.

---

## 📜 License

This project is **proprietary** — see the author for commercial licensing inquiries.

The underlying frameworks are licensed under their respective terms:
- **nih-plug**: ISC License
- **HTDemucs model**: MIT License (Meta Research)
- **ONNX Runtime**: MIT License

---

## 🙏 Credits

- **Plugin framework**: [nih-plug](https://github.com/robbert-vdh/nih-plug) by Robbert van der Helm
- **UI framework**: [egui](https://github.com/emilk/egui) by Emil Ernerfeldt
- **AI model**: [HTDemucs](https://github.com/facebookresearch/demucs) by Meta Research
- **Audio decoding**: [Symphonia](https://github.com/pdeljanov/Symphonia)

---

<div align="center">

**StemSampler Pro** — *Where AI meets craft.*

[Report Bug](https://github.com/yourusername/stem_sampler/issues) • [Request Feature](https://github.com/yourusername/stem_sampler/issues) • [Discussions](https://github.com/yourusername/stem_sampler/discussions)

</div>
