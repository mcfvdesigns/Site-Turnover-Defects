3 False Positive Cases :
1. Surface Shadows & Lighting Artifacts Misidentified as moisture stain
  What Happened: The model placed a high-confidence bounding box around dark, non-uniform ambient shadows in wall corners and ceiling joins, flagging them as moisture stain.

  Why It Happened: Moisture stains in construction imagery are characterized by soft-edged, irregular dark patches. Sharp surface shadows cast by overhead site lighting exhibit identical color gradients and low contrast values, confusing the model due to a lack of spatial lighting context.

2. Surface Dirt and Dry Scuff Marks Misidentified as paint defect
  What Happened: Dry construction dust, shoe scuffs, or smudges on drywall finishes were misclassified as paint defect.

  Why It Happened: The model learned to detect localized color variations and texture irregularities on smooth painted surfaces. Because the training set lacks negative examples of benign surface dirt, any localized discoloration on a light surface gets flagged as a paint failure.

3. Tile Grout Line Depth Inconsistencies Misidentified as uneven joint
  What Happened: Normal tile grout lines captured under directional or low-angle lighting were flagged as uneven joint defects.

  Why It Happened: Directional lighting creates a dark shadow along one side of a standard grout line. The model misinterprets this linear shadow contrast as an alignment offset or width variation between adjacent tiles.

3 False Negative Cases
1. Hairline Cracks on Door Frame Substrates Missed as cracked jambWhat Happened: Fine, thin hairline fractures along painted timber or aluminum door jambs went completely undetected by the model.
   Why It Happened: Standard YOLO resize operations downsample high-resolution inspection photos down to 640 \times 640 pixels. Micro-features like sub-millimeter cracks lose spatial edge sharpness and pixel contrast during feature extraction, causing confidence scores to fall below the default detection threshold.
2. Minor Edge Chips in Dark Surrounds Omitted as cracked tile
   What Happened: Small chips along tile corners and grout edges in dimly lit room corners were missed.
   Why It Happened: Low contrast in shadow-heavy regions prevents the feature extraction backbone from distinguishing dark ceramic chips from surrounding dark grout lines. Without dedicated dynamic contrast augmentation (e.g., CLAHE), low-light surface variations fail to trigger bounding box generation.
3. Faint Water Rings and Seepage Lines Omitted as moisture stain
   What Happened: Light, dried water marks and subtle ring discoloration on light-colored ceiling boards were not detected.
   Why It Happened: The boundary gradients of faint water stains are extremely subtle and blend into surrounding painted textures. Because the model was predominantly exposed to dark, high-contrast moisture damage during training, it fails to recognize low-saturation, low-contrast boundary patterns as active defects.

3 Actionable Data Improvements
   1. High-Resolution Sub-Crop Training for Micro-Defects
      Action: Implement a tiled or sliding-window cropping pipeline for high-resolution site photos before resizing.
      Why It Helps: Extracting 640 \times 640 patches around small defects (like hairline cracks on door jambs or chipped tile corners) preserves native pixel sharpness and fine edge detail, preventing micro-features from getting blurred out during standard YOLO resizing.

   2. Specialized Contrast & Lighting Augmentations (CLAHE)
       Action: Add dynamic Contrast Limited Adaptive Histogram Equalization (CLAHE), random brightness/contrast shifts, and shadow simulation to the training augmentation pipeline.
       Why It Helps: Exposing the model to extreme lighting variation teaches the convolutional filters to distinguish true texture boundaries (such as moisture ring boundaries) from ambient environmental shadows and low-light corner occlusions.
   
   3. Balanced Class Sampling & Negative Background Injection
      Action: Collect additional training images for under-represented classes (cracked jamb, cracked tile) and incorporate explicit "background-only" images containing benign surface imperfections (e.g., normal tile grout lines, dry scuff marks, dust).
      Why It Helps: Adding non-defect background samples suppresses false positive detections on benign site dirt and grout shadows, while balancing the dataset prevents the model from favoring over-represented classes.
   
