# Ossur Prosthetic Classifier

A fast.ai-powered image classifier that identifies different types of Ossur prosthetic feet and blades. This project was built as part of my AI builder journey to create tools that solve real prosthetic challenges.

## 🎯 Project Overview

This classifier can distinguish between three categories of Ossur prosthetics:
- Daily Walking Foot - For everyday mobility and comfort
- Extreme Sport Blade - For high-performance athletic activities  
- Multi-Purpose Foot - For versatile, active lifestyles

## 📊 Results

- *raining Accuracy: 100% (0% error rate achieved in epoch 0)
- Test Predictions: 100% accuracy across all categories
- Confidence Scores: 81-95% on test images
- Model Size: Compact .pkl file ready for deployment

## 🗂️ Data Collection Method

### Manual Collection Process
- Source: Official Ossur website (www.ossur.com)
- Method: Manual download of product images
- Total Images: 28 high-quality product shots
- Categories: 
  - Daily Walking Foot: 8 images
  - Extreme Sport Blade: 8 images  
  - Multi-Purpose Foot: 12 images

### Data Organization
- Created systematic folder structure for fast.ai compatibility
- Used Excel spreadsheet to track image metadata:
  - Product names, activity levels, waterproof ratings
  - URL sources for traceability
  - Category classifications

## 🚧 Difficulties Faced & Solutions

### 1. Kaggle Dataset Upload Issues
- Problem: Double folder structure (`/prosthetic-classifier/prosthetic-classifier/`)
- Solution: Updated path to include extra folder level
- Lesson: Always verify exact file paths in cloud environments

### 2. DataLoader Batch Size Errors
- Problem: ValueError: This DataLoader does not contain any batches`
- Root Cause: Batch size (32) larger than dataset size (28 images)
- Solution: Reduced batch size to 8 for small dataset
- Lesson: Batch size must be appropriate for dataset size

### 3. ast.ai Folder Structure Requirements
- Problem: Images not recognized by DataBlock
- Solution: Learned fast.ai expects specific structure:
  ```
  dataset/
  ├── category1/
  ├── category2/
  └── category3/
  ```
- Lesson: Framework-specific requirements matter

### 4. Path Configuration Confusion
- Problem: Multiple path variables causing conflicts
- Solution: Systematic debugging with `path.exists()` and `get_image_files()`
- Lesson: Always verify data accessibility before training

## 🛠️ Technical Implementation

### Model Architecture
- Base Model: ResNet18 (pre-trained)
- Framework: fast.ai vision_learner
- Training: Transfer learning with 3 epochs
- Validation Split: 80/20 random split (seed=42)

### Key Code Components
```python
# DataBlock configuration
dls = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=[Resize(192, method='squish')]
).dataloaders(path, bs=8)

# Model training
learn = vision_learner(dls, resnet18, metrics=error_rate)
learn.fine_tune(3)
```

## 📈 Future Improvements

### Short-term (Next Month)
1. Expand Dataset
   - Target: 50-100 images per category
   - Include real-world photos (not just product shots)
   - Add different angles and lighting conditions

2. Enhanced Categories
   - Split multi-purpose into subcategories
   - Add ankle units vs feet-only classification
   - Include activity-level granularity

### Medium-term (2-3 Months)
3. Multi-Brand Support
   - Add Ottobock, Freedom Innovations, College Park
   - Retrain as "Universal Prosthetic Classifier"
   - Compare cross-brand accuracy

4. Production Features
   - Confidence thresholds for uncertain predictions
   - Price range integration from companion Excel data
   - API endpoints for third-party integration

### Long-term (6+ Months)
5. Advanced Capabilities
   - Custom fitting recommendations
   - Integration with prosthetic pricing tools
   - Real-time inventory checking
   - Mobile app deployment

## 🎯 Real-World Impact

This classifier addresses genuine prosthetic user challenges:
- Education: Helps users understand prosthetic options
- Selection: Assists in choosing appropriate activity-level devices
- Research: Enables analysis of prosthetic market trends

As someone who lost a leg in 2020, this project represents my commitment to building AI tools that solve real prosthetic challenges and help others like me navigate their options more effectively.

## 🚀 Deployment

Model exported as `prosthetic_classifier.pkl` and ready for:
- Gradio web interface
- HuggingFace Spaces deployment  
- Integration into larger prosthetic platforms

## 📝 Technical Notes

- Framework: fast.ai 2.x
- Environment: Kaggle Notebooks
- Training Time: ~3 minutes on Kaggle GPU
- Model File: 87MB exported .pkl
- Dependencies: fastai, torch, gradio

---

Built with fast.ai as part of Chapter 1 completion in my full-time AI builder journey.
