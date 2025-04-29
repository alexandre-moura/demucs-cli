# Demucs-CLI — AI Stem-splitting Pipeline

Demucs-based high-fidelity stem separation tool built for convenient CLI usage.

## Project Overview

Demucs-CLI provides a one-line CLI tool to separate full songs into high-quality stems (vocals, drums, bass, others) using the highest quality `htdemucs_ft` model.
It automates virtualenv activation, performance tuning, and audio preprocessing — maximum quality is the goal.

**Warning**:
Precautions were taken to avoid hardware overexhertion, but are highly GPU-architecture depedent. **System monitoring is advised.**

## Features

- One-line global command (`demucs-boost`) to split songs anywhere
- High-fidelity model (`htdemucs_ft`, bag of 4 Transformer models)
- Full CLI integration with automatic environment handling
- Preprocessing with FFmpeg for cleaner input signals
- Parallel processing (`-j` flag) for faster chunk inference

## Project Structure

| File           | Purpose                              |
| -------------- | ------------------------------------ |
| `demucs-boost` | Main CLI script wrapper              |
| `.gitignore`   | Standard ignores (audio, temp files) |
| `README.md`    | Project documentation                |
| `requirements.txt`| Necessary libs                    |

## How to Use

### Install Dependencies

Make sure you have Python 3.10+, FFmpeg, and git installed.

Clone the repository:

```bash
git clone https://github.com/yourusername/demucs-cli.git
cd demucs-cli
```

Create and activate a virtual environment:

```bash
python3 -m venv demucs_env
source demucs_env/bin/activate
pip install -r requirements.txt
brew install ffmpeg  # or use your package manager
```

Make it globally accessible

```bash
chmod +x demucs-boost
ln -s $(pwd)/demucs-boost /usr/local/bin/demucs-boost
```

### Run Separation

```bash
./demucs-boost "/path/to/your/audio.mp3"
```

The output will be saved in the same folder by default, or you can specify a custom output path.

## Example

Input:

```
Ghost - Kiss The Go-Goat (Official Audio).mp3
```

Command:

```bash
demucs-boost "Ghost - Kiss The Go-Goat (Official Audio).mp3"
```

Output:

```
demucs_output/
└── htdemucs_ft/
    ├── vocals.wav
    ├── drums.wav
    ├── bass.wav
    └── other.wav
```

## Technologies Used

- Python 3.10
- Demucs v4.0.1
- Shell scripting (ZSH/Bash)
- PyTorch (MPS backend awareness)
- FFmpeg (format handling)

## Performance Notes and Observations

- `htdemucs_ft` delivers highest-fidelity and least artifact presence for voice and instrumentals among `htdemucs`, `htdemucs_ft`, and `mdx_extra_q` across all tested settings.
- `demucs-boost` preset script automatically maxes all relevant parameters (within hardware limits), including shifts, overlap, bitrate handling, and output directory.
- GPU acceleration (`-d mps`) remains unsupported on Apple Silicon PyTorch MPS backend limitations; all operations default to CPU.
- Running with maximum settings is very resource-intensive and slow; CPU multithreading is supported under the `-j` flag, but caution is advised.

## Comparative Results

- UVR **(Ultimate Vocal Remover - multi-model GUI)** offers slightly better vocal isolation but worse instrumental separation compared to Demucs. Worth checking out especially for karaoke setup.
- If you want _even higher quality_ stem-splitting or are running into time / hardware limitations, **Logic Pro**'s built-in tool (Demucs finetuning) offers better results and runs about 100x faster.

## License

MIT License
