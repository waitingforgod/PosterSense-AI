# PosterSense-AI
A showcase project for AI-powered movie poster understanding visual analysis and smart similarity search


PosterSense AI 可以对用户上传的电影海报进行 类型预测、视觉风格分析和相似海报检索，并通过 Streamlit 提供交互式 Web 界面。

功能

1. 电影类型预测

使用 EfficientNet-B0 对电影海报进行多标签分类，目前支持：

Drama / Comedy / Action / Romance / Crime / Thriller / Adventure / Horror / Fantasy / Sci-Fi

示例：

Drama      85.3%
Romance    81.7%
Comedy     61.5%

2. Poster DNA

分析海报的主要视觉特征：

主色调

亮度

对比度

色彩丰富度

冷暖色调

综合视觉风格

示例：

Brightness:   58
Contrast:     52
Colorfulness: 82

Style:
Balanced · Colorful · Cool Tone

3. Smart Match

使用 CLIP ViT-B/32 提取海报视觉特征，并结合 Genre 信息进行二阶段检索：

上传海报
   ↓
CLIP Visual Retrieval
   ↓
Top-100 Candidates
   ↓
Genre Re-ranking
   ↓
Top-5 Similar Posters

当前综合评分：

Smart Score
= 0.75 × Visual Score
+ 0.25 × Genre Score

系统流程

1. Upload a movie poster.
2. EfficientNet-B0 predicts the most likely movie genres.
3. CLIP extracts visual features and retrieves similar posters.
4. Poster DNA analyzes color, brightness, contrast and visual style.
5. All results are integrated and displayed in the Streamlit interface.

数据集

使用 Kaggle Movie Posters 数据集：

清洗后用于 Genre 分类的数据：

总样本：6662
Train：5329
Validation：666
Test：667

相似海报检索库：

7242 张海报
每张海报：512维 CLIP Embedding

模型表现

最佳模型：

Model: EfficientNet-B0
Best Epoch: 2
Best Validation Loss: 0.9129

Test Set：

Metric

Score

Precision

0.3890

Recall

0.7162

Micro F1

0.5042

Macro F1

0.4460

当前模型更偏向较高 Recall，因此 Web 页面展示 Top-3 Genre Probability，而不是简单的 Yes / No 分类。


安装

pip install torch torchvision
pip install pandas numpy pillow scikit-learn
pip install open-clip-torch tqdm streamlit

使用

1. 数据预处理


2. 训练 Genre 模型



3. 构建 CLIP Embedding



4. 启动 Web App



浏览器访问



技术栈

Python

PyTorch

EfficientNet-B0

OpenCLIP

NumPy

scikit-learn

Pillow

Streamlit

当前限制

Genre Prediction 只依赖海报视觉信息；

部分类别存在数据不平衡；

Poster DNA 为视觉描述指标，不是审美评分；

Smart Match 当前使用固定的 Visual / Genre 权重；

CPU 环境下首次加载 CLIP 会稍慢。
