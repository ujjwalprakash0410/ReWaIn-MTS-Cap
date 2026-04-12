Traffic sign detection and recognition have seen
significant progress through deep learning. However, generating natural language descriptions of traffic signs—a capability
critical for explainable driver assistance systems and semantic
road scene understanding—remains largely unexplored, particularly for Latin American road infrastructure. In this work, we
introduce ReWaIn-MTS-Cap, the first vision-language dataset
and benchmark for Mexican traffic signs. Building upon the
existing ReWaIn-MTS detection dataset (Bolanos-Flores et al., ˜
2025), which provides 2,283 bounding-box annotated traffic sign
instances across 37 classes from 8 Mexican cities, we extend it
by authoring 5 structured natural language captions per sign
instance, yielding approximately 11,415 total caption pairs. Each
caption template captures a distinct semantic perspective: sign
identification and meaning, scene context and mounting description, driver instruction, regulatory framing, and sign category
with visual characteristics. We present a two-phase system: Phase
1 employs YOLOv11 for traffic sign detection, achieving 95.97%
mAP@50 on the ReWaIn-MTS dataset, surpassing the original
paper’s reported result of 92.64% despite training for only 25
epochs. Phase 2 fine-tunes BLIP (Bootstrapping Language-Image
Pre-training) using a crop-based training strategy with class
balancing to generate natural language captions for individual
sign regions. Our fine-tuned BLIP model achieves BLEU-4 =
0.510 and CIDEr = 1.775, substantially exceeding the BLIP
COCO baseline (BLEU-4 = 0.403, CIDEr = 1.330) and outperforming all compared state-of-the-art traffic sign captioning
methods. The ReWaIn-MTS-Cap dataset, trained models, and
evaluation code will be made publicly available to support further
research in vision-language understanding for Latin American
road infrastructure.
