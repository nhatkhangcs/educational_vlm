# Pixels to Prompts

A set of Jupyter notebooks that teach how vision-language models work, by building the pieces in code. We start with how an image becomes a vector and finish by training a small LLaVA-style model that can answer questions about photos.

The notebooks go along with the book [*From Pixels to Prompts: Vision-Language Models*](https://arxiv.org/abs/2605.07544), which covers the same ideas in more depth.

You should know some Python and have used PyTorch a little. You don't need a background in computer vision or NLP.

## The notebooks

Work through them in order, since each one builds on the one before.

1. **`01_images_become_vectors.ipynb`** takes one CIFAR-10 image and follows it through CLIP's vision encoder: preprocessing, cutting it into patches, the transformer, and the final 512-number vector.
2. **`02_text_becomes_vectors.ipynb`** does the same for text. It tokenizes a few AG News articles with CLIP's tokenizer and turns each one into a vector.
3. **`03_image_text_similarity.ipynb`** scores four COCO photos against their captions with a pretrained CLIP, and rebuilds CLIP's training loss line by line from the paper.
4. **`04_miniclip_from_scratch.ipynb`** builds a small CLIP, trains it from scratch on 800 image–caption pairs, and checks what it learned.
5. **`05_zero_shot_classification.ipynb`** classifies CIFAR-10 images without any training, by comparing each image with sentences like "A photo of a cat."
6. **`06_image_text_retrieval.ipynb`** searches a small photo catalog with a caption, searches captions with a photo, and measures both with Recall@K.
7. **`07_captioning_with_cross_attention.ipynb`** trains a tiny captioning model that reads image patches through cross-attention and writes a caption one word at a time.
8. **`08_mini_vqa.ipynb`** answers questions about images by feeding CLIP's image and question vectors into a small classifier.
9. **`09_llava_style_connector.ipynb`** builds a mini LLaVA. It connects CLIP to a small Qwen language model with a single linear layer and trains it in two stages, so it can answer questions about images in its own words.

## How each notebook is laid out

Every section starts with a question, like "How does CLIP cut the image into pieces?". Then comes a short piece of code that answers it, and then an explanation of what the output means: the shape of a tensor, a score, a loss, or a ranking. When a notebook is based on a paper, it links to the paper and uses its figures to walk through the code step by step.

Along the way you'll see:

- **Pause and predict** prompts, which ask you to guess the result before you run the next cell.
- **TODO** cells, where you change a value (a different image, your own caption, another prompt) and run the cell again to see what changes. They work with their default values, so you can skip them on a first pass.
- At the end of each notebook, a **Recap**, a few **Check your understanding** questions with hidden answers, and a **Try it yourself** section with a few longer TODO exercises.

## Getting started

```bash
pip install -r requirements.txt
jupyter notebook
```

Open `notebooks/01_images_become_vectors.ipynb` and run it from the top. The first run of each notebook downloads its dataset and the pretrained weights from Hugging Face. After that, they load from your local cache.

The first code cell in every notebook sets `GPU_ID`. Leave it at `"0"` if you have one GPU, change it to use a different one (`nvidia-smi` lists them), or set it to `""` to run on the CPU. Notebooks 01–08 all run on a CPU. Most take under a minute, but the two that train a model are slower: on our machine, 04 took about 18 minutes and 07 about 5.

Notebook 09 is much bigger than the rest. It needs a GPU, downloads about 13.5 GB of VQAv2, caches about 6.2 GB of image features, and takes several hours to train. Plan for 35–50 GB of free disk space.

## Datasets

All data comes from the Hugging Face Hub through the `datasets` library, with pinned revisions so every run sees the same rows.

- CIFAR-10 (`uoft-cs/cifar10`) in notebooks 01 and 05
- AG News (`fancyzhx/ag_news`) in notebook 02
- A small COCO captions subset (`Erland/coco_captions_small`) in notebooks 03, 04, 06, 07, and 09
- A small VQAv2 validation sample in notebook 08, and as the held-out set in notebook 09
- The full VQAv2 training split (443,757 questions) from `HuggingFaceM4/the_cauldron` in notebook 09

## Project layout

```text
notebooks/        the nine lessons
assets/diagrams/  figures used in the notebooks, including figures from the papers
requirements.txt
```

If you change a notebook, please clear its outputs before committing so the next reader starts fresh.

## Reference

If you use these notebooks or the book, please cite:

```bibtex
@misc{vo2026pixels,
  title         = {From Pixels to Prompts: Vision-Language Models},
  author        = {Vo, Khang Nhat Hoang},
  year          = {2026},
  eprint        = {2605.07544},
  archivePrefix = {arXiv},
  url           = {https://arxiv.org/abs/2605.07544}
}
```
