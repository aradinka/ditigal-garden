---
title: SpectralGPT
---
# Key takeaway

- Universal remote sensing foundation model
- Trained on one million spectral remote sensing Sentinel-2 images
- 3 models (Million Parameters): Base (100M), large, (300M) huge (600M)
- 3D masking strategy, encoder for learning visual representation from spatial-spectral mixed tokens, decoder with multi-target reconstruction for preserving spectrally sequential characteristics
- Masked autoencoder (MAE) framework, masking rate 90%
- New benchmark dataset SegMunich, focuses on urban areas
- Pretrained: 700k data 12 spectral bands from fMoW-S2 200 epoch, 350k BigEarthNet Sentinel-2 100 epoch. 8 NVIDIA RTX 4090 GPUs for training, 4 gpus for finetuning

# Downstream task

- single/multi-label scene classification: EuroSAT S2 images 10 land classes 13 bands 3k labeled images, 150 epochs
- semantic segmentation
- change detection

# Prev works

- Momentum contrast (MoCo): Introduce momentum updates to improve contrastive learning process
- Simple contrastive learning (SimCLR): Leverages data augmentations to enhance the variety and complexity of the image pairs used for contrastive learning
- Generative learning based on masked image modeling (MIM)
	- Example, MIM architecture: Bidirectional encoder representation from image transformer (BEiT) built on top of vision transformers (ViT). Also MIM allows for flexible use of various deep architectures as network backbones (ViT, Swin Transformers)
	- MIM allows input of all images patches, high computational cost, limited for certain application
	- MIM alternative: Masked autoencoders (MAE), unmasked patcher or pixels used to reconstruct those that are masked. Computationally more efficient
- SatMAE: Pre-training transformers for temporal and multi-spectral satellite imagery
- fMoW-S2 dataset: Training from scratch 700k images, 3D tensor-based random weight initialization
- BigEarthNet-S2 dataset: Progressive pre training on 350k images, varying image sizes, resolution, time series information, and geographic regions. Fine tuning data 35k images with labels

