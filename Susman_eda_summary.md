# EDA Summary — Road Traffic Sign Recognition

**Student:** Susman | **Course:** TECH 405 | **Task:** Exploratory Data Analysis

The dataset ("Self-Driving Cars", Roboflow Universe, CC BY 4.0) contains 4,969 images, fixed at
416×416 RGB by the exporter, annotated in YOLO object-detection format with 15 classes (traffic
lights, speed-limit signs 10–120, and Stop), totaling 6,012 bounding-box instances. Because images
can contain multiple signs, each annotated box was treated as one labeled sample for
classification-style analysis.

**Data-quality checks** found no corrupt files, no missing labels, and no non-RGB images, but
uncovered 996 degenerate bounding boxes (<8px, excluded from analysis) and — most importantly —
230 groups of exact duplicate images, 101 of which span across the train/valid/test split,
meaning 202 images currently leak between splits and would inflate reported validation/test
accuracy.

**Class distribution** is imbalanced: Green Light has 468 instances versus only 17 for Speed
Limit 10 (27.5:1 ratio), so class-weighted loss and targeted augmentation are recommended.

**Image characteristics:** raw frames are uniformly 416×416, but cropped signs range from 8px to
416px (median ≈152×181px). A target resolution of **96×96** is recommended, based on the median
largest bounding-box side (~203px), balancing digit-level detail against compute cost.

**Split strategy:** a stratified 70/15/15 train/validation/test split (after deduplication) is
recommended over the existing split, to keep each class proportionally represented given the
imbalance.

**Three key findings:** (1) duplicate images leak across the existing split, undermining
evaluation reliability; (2) severe class imbalance (27.5:1) will bias the model toward frequent
classes; (3) the dataset is detection-style, not pre-cropped — mean-image analysis shows all
speed-limit signs share an identical red-circle template, so the model must resolve fine digit
detail, reinforcing the need for a resolution that preserves it and for duplicate/degenerate-box
cleanup before training.

No model was trained; this was EDA only.
