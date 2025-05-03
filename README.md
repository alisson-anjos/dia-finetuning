## Usage:
```bash
git clone https://github.com/stlohrey/dia-finetuning.git
cd dia-finetuning
pip install -e .
```

then


```bash
python -m dia.finetune \
  --config path/to/dia/config.json \
  --dataset Paradoxia/opendata-iisys-hui \
  --hub_model nari-labs/Dia-1.6B \
  --run_name my_experiment \
  --output_dir ./checkpoints
```
or
```bash
python -m dia.finetune \
	--local_ckpt "/home/alissonerdx/projects/dia-finetuning/checkpoints/10000_steps_epoch_1_AdrienB134portuguese-tts.pth"
	--config config.json \
	--csv_path "/home/alissonerdx/projects/dia-finetuning/podcast.csv" \
	--audio_root "/home/alissonerdx/projects/dia-finetuning" \
	--run_name podcast \
	--output_dir ./checkpoints_2 \
	--epochs 10 \
	--learning_rate 0.00002
```

# 🗣️ Whisper + Diarization Transcription Script

This script allows you to automatically **transcribe and diarize** audio files using OpenAI Whisper and PyAnnote. It supports:
- Segmenting long audio into chunks.
- Speaker labeling with `[S1]`, `[S2]`, etc.
- Using pre-segmented folders of `.wav` files.
- Reading `.txt` transcripts (if they already exist).
- Writing results incrementally to a CSV.
- Optionally preserving or overwriting `.txt` files.

## 🔧 Requirements

Install the required packages:
```bash
pip install torch torchaudio transformers pyannote.audio librosa pandas tqdm
```

To use diarization, you must:
- Install `pyannote.audio==3.1` via `pip install pyannote.audio`
- Accept access conditions for:
  - `pyannote/segmentation-3.0`
  - `pyannote/speaker-diarization-3.1`
- Create and use a Hugging Face access token from: https://hf.co/settings/tokens

## 🚀 Usage Examples

### 1. Process and segment a long audio file:
```bash
python prepare-dataset.py [audio file here] \
  --output_dir podcasts \
  --csv_path podcast.csv \
  --append_csv \
  --segment_length 30 \
  --hf_token [HF token] \
  --trim_start 60 \
  --trim_end 60
```

### 2. Process a folder of pre-segmented `.wav` files (non-recursive):
```bash
python prepare-dataset.py --csv_path podcast2.csv \
  --append_csv \
  --hf_token [HF token] \
  --from_segments Ieda_F014 \
  --include_original
```

### 3. Recursively transcribe `.wav` files in nested folders:
```bash
python prepare-dataset.py --from_segments path/to/main_folder \
  --csv_path output.csv \
  --hf_token [HF token]
```

## 🧠 Optional Flags

| Flag                          | Description |
|-------------------------------|-------------|
| `--include_original`          | Adds the content of each `.txt` (if exists) to the CSV as `original_text` |
| `--skip_existing`             | Skips `.wav` files that already have a `.txt` file |
| `--add_diarization_to_existing` | Processes existing `.txt` files that lack `[Sx]` speaker tags and writes the updated diarized version to the CSV (original `.txt` is not modified unless `--overwrite_txt` is set) |
| `--overwrite_txt`             | Allows overwriting `.txt` files with new diarized transcriptions |
| `--append_csv`                | Appends to the existing CSV instead of overwriting |
| `--trim_start`, `--trim_end`  | Trims seconds from the start/end of long audio files before segmenting |
| `--segment_length`            | Sets segment duration in seconds (default: 30s) |

## 📝 Output

- A `.csv` file with one row per audio segment:
  - `audio`: path to the segment
  - `text`: diarized transcription
  - `original_text`: original `.txt` content (if `--include_original` is set)

- `.txt` files with the transcription will be created next to each `.wav` unless `--skip_existing` is used.


## Original readme

<p align="center">
<a href="https://github.com/nari-labs/dia">
<img src="./dia/static/images/banner.png">
</a>
</p>
<p align="center">
<a href="https://tally.so/r/meokbo" target="_blank"><img alt="Static Badge" src="https://img.shields.io/badge/Join-Waitlist-white?style=for-the-badge"></a>
<a href="https://discord.gg/pgdB5YRe" target="_blank"><img src="https://img.shields.io/badge/Discord-Join%20Chat-7289DA?logo=discord&style=for-the-badge"></a>
<a href="https://github.com/nari-labs/dia/blob/main/LICENSE" target="_blank"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=for-the-badge" alt="LICENSE"></a>
</p>
<p align="center">
<a href="https://huggingface.co/nari-labs/Dia-1.6B"><img src="https://huggingface.co/datasets/huggingface/badges/resolve/main/model-on-hf-lg-dark.svg" alt="Dataset on HuggingFace" height=42 ></a>
<a href="https://huggingface.co/spaces/nari-labs/Dia-1.6B"><img src="https://huggingface.co/datasets/huggingface/badges/resolve/main/open-in-hf-spaces-lg-dark.svg" alt="Space on HuggingFace" height=38></a>
</p>

Dia is a 1.6B parameter text to speech model created by Nari Labs.

Dia **directly generates highly realistic dialogue from a transcript**. You can condition the output on audio, enabling emotion and tone control. The model can also produce nonverbal communications like laughter, coughing, clearing throat, etc.

To accelerate research, we are providing access to pretrained model checkpoints and inference code. The model weights are hosted on [Hugging Face](https://huggingface.co/nari-labs/Dia-1.6B). The model only supports English generation at the moment.

We also provide a [demo page](https://yummy-fir-7a4.notion.site/dia) comparing our model to [ElevenLabs Studio](https://elevenlabs.io/studio) and [Sesame CSM-1B](https://github.com/SesameAILabs/csm).

- (Update) We have a ZeroGPU Space running! Try it now [here](https://huggingface.co/spaces/nari-labs/Dia-1.6B). Thanks to the HF team for the support :)
- Join our [discord server](https://discord.gg/pgdB5YRe) for community support and access to new features.
- Play with a larger version of Dia: generate fun conversations, remix content, and share with friends. 🔮 Join the [waitlist](https://tally.so/r/meokbo) for early access.

## ⚡️ Quickstart

### Install via pip

```bash
# Install directly from GitHub
pip install git+https://github.com/nari-labs/dia.git
```

### Run the Gradio UI

This will open a Gradio UI that you can work on.

```bash
git clone https://github.com/nari-labs/dia.git
cd dia && uv run app.py
```

or if you do not have `uv` pre-installed:

```bash
git clone https://github.com/nari-labs/dia.git
cd dia
python -m venv .venv
source .venv/bin/activate
pip install -e .
python app.py
```

Note that the model was not fine-tuned on a specific voice. Hence, you will get different voices every time you run the model.
You can keep speaker consistency by either adding an audio prompt (a guide coming VERY soon - try it with the second example on Gradio for now), or fixing the seed.

## Features

- Generate dialogue via `[S1]` and `[S2]` tag
- Generate non-verbal like `(laughs)`, `(coughs)`, etc.
  - Below verbal tags will be recognized, but might result in unexpected output.
  - `(laughs), (clears throat), (sighs), (gasps), (coughs), (singing), (sings), (mumbles), (beep), (groans), (sniffs), (claps), (screams), (inhales), (exhales), (applause), (burps), (humming), (sneezes), (chuckle), (whistles)`
- Voice cloning. See [`example/voice_clone.py`](example/voice_clone.py) for more information.
  - In the Hugging Face space, you can upload the audio you want to clone and place its transcript before your script. Make sure the transcript follows the required format. The model will then output only the content of your script.

## ⚙️ Usage

### As a Python Library

```python
import soundfile as sf

from dia.model import Dia


model = Dia.from_pretrained("nari-labs/Dia-1.6B")

text = "[S1] Dia is an open weights text to dialogue model. [S2] You get full control over scripts and voices. [S1] Wow. Amazing. (laughs) [S2] Try it now on Git hub or Hugging Face."

output = model.generate(text)

sf.write("simple.mp3", output, 44100)
```

A pypi package and a working CLI tool will be available soon.

## 💻 Hardware and Inference Speed

Dia has been tested on only GPUs (pytorch 2.0+, CUDA 12.6). CPU support is to be added soon.
The initial run will take longer as the Descript Audio Codec also needs to be downloaded.

On enterprise GPUs, Dia can generate audio in real-time. On older GPUs, inference time will be slower.
For reference, on a A4000 GPU, Dia roughly generates 40 tokens/s (86 tokens equals 1 second of audio).
`torch.compile` will increase speeds for supported GPUs.

The full version of Dia requires around 12-13GB of VRAM to run. We will be adding a quantized version in the future.

If you don't have hardware available or if you want to play with bigger versions of our models, join the waitlist [here](https://tally.so/r/meokbo).

## 🪪 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## ⚠️ Disclaimer

This project offers a high-fidelity speech generation model intended for research and educational use. The following uses are **strictly forbidden**:

- **Identity Misuse**: Do not produce audio resembling real individuals without permission.
- **Deceptive Content**: Do not use this model to generate misleading content (e.g. fake news)
- **Illegal or Malicious Use**: Do not use this model for activities that are illegal or intended to cause harm.

By using this model, you agree to uphold relevant legal standards and ethical responsibilities. We **are not responsible** for any misuse and firmly oppose any unethical usage of this technology.

## 🔭 TODO / Future Work

- Docker support.
- Optimize inference speed.
- Add quantization for memory efficiency.

## 🤝 Contributing

We are a tiny team of 1 full-time and 1 part-time research-engineers. We are extra-welcome to any contributions!
Join our [Discord Server](https://discord.gg/pgdB5YRe) for discussions.

## 🤗 Acknowledgements

- We thank the [Google TPU Research Cloud program](https://sites.research.google/trc/about/) for providing computation resources.
- Our work was heavily inspired by [SoundStorm](https://arxiv.org/abs/2305.09636), [Parakeet](https://jordandarefsky.com/blog/2024/parakeet/), and [Descript Audio Codec](https://github.com/descriptinc/descript-audio-codec).
- Hugging Face for providing the ZeroGPU Grant.
- "Nari" is a pure Korean word for lily.
- We thank Jason Y. for providing help with data filtering.


## ⭐ Star History

<a href="https://www.star-history.com/#nari-labs/dia&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=nari-labs/dia&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=nari-labs/dia&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=nari-labs/dia&type=Date" />
 </picture>
</a>
