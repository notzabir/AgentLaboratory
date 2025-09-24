# VLM (Vision-Language Model) Integration Guide for Agent Laboratory

## Overview

Agent Laboratory now supports DeepSeek VLM (Vision-Language Models) that can process both text and images. This integration allows researchers to conduct experiments involving visual analysis, image understanding, and multimodal reasoning.

## Supported VLM Models

### DeepSeek VLM Models
- **deepseek-vl-7b-chat**: 7B parameter vision-language model
  - Higher quality visual understanding
  - Better reasoning capabilities
  - Suitable for complex visual analysis tasks

- **deepseek-vl-1.3b-chat**: 1.3B parameter vision-language model
  - Faster inference
  - Lower cost
  - Good for simpler visual tasks

## Setup and Configuration

### 1. API Setup
```bash
export DEEPSEEK_API_KEY="your-deepseek-api-key-here"
```

### 2. YAML Configuration
Use the VLM example configuration:
```yaml
llm-backend: "deepseek-vl-7b-chat"  # or "deepseek-vl-1.3b-chat"
deepseek-api-key: "your-api-key-here"
```

### 3. Using VLM in Code
```python
from utils import query_deepseek_vlm

# Analyze an image with text prompt
response = query_deepseek_vlm(
    prompt="What do you see in this image? Describe the main elements.",
    system="You are an expert image analyst.",
    api_key=os.getenv('DEEPSEEK_API_KEY'),
    image_path="path/to/your/image.jpg",
    model="deepseek-vl-7b-chat"
)
```

## Cost Analysis

### API Costs (DeepSeek VLM)
- **Input tokens**: ~$1.50 per 1M tokens (deepseek-vl-7b-chat)
- **Output tokens**: ~$7.50 per 1M tokens (deepseek-vl-7b-chat)
- **Input tokens**: ~$0.80 per 1M tokens (deepseek-vl-1.3b-chat)
- **Output tokens**: ~$4.00 per 1M tokens (deepseek-vl-1.3b-chat)

**Image Processing**: Images are converted to tokens, typically:
- Small image (512x512): ~100-300 tokens
- Medium image (1024x1024): ~400-800 tokens
- Large image (2048x2048): ~1000-2000 tokens

**Estimated costs per image analysis**:
- Simple analysis: $0.01-0.05 per image
- Complex analysis: $0.05-0.20 per image

### Local Deployment Costs

#### Hardware Requirements
- **GPU Memory**: 8GB+ VRAM (for 7B model), 4GB+ VRAM (for 1.3B model)
- **RAM**: 16GB+ system RAM recommended
- **Storage**: 15GB+ for model files

#### Local vs API Decision Matrix

| Factor | Local Deployment | API Usage |
|--------|------------------|-----------|
| **Initial Cost** | $500-2000 (GPU) | $0 |
| **Per-image Cost** | ~$0.001 (electricity) | $0.01-0.20 |
| **Break-even Point** | ~1000-10000 images | N/A |
| **Setup Complexity** | High | Low |
| **Scalability** | Limited by hardware | High |
| **Privacy** | Full control | Shared service |
| **Model Updates** | Manual | Automatic |

### Recommendations

#### Use API When:
- Processing < 1000 images per month
- Need latest model versions
- Want zero setup complexity
- Prefer pay-per-use model
- Privacy is not a major concern

#### Use Local When:
- Processing > 10000 images per month
- Have privacy/security requirements
- Already have suitable GPU hardware
- Want predictable costs
- Need offline capability

## Usage Examples

### 1. Scientific Figure Analysis
```python
# Analyze a research paper figure
response = query_deepseek_vlm(
    prompt="Analyze this scientific figure. What type of chart is it? What are the main findings?",
    system="You are a scientific research assistant expert in data visualization.",
    image_path="research_figure.png",
    model="deepseek-vl-7b-chat"
)
```

### 2. Chart Data Extraction
```python
# Extract data from charts
response = query_deepseek_vlm(
    prompt="Extract the numerical data from this chart and format it as a table.",
    system="You are a data extraction expert.",
    image_path="chart.jpg",
    model="deepseek-vl-1.3b-chat"  # Faster for simple tasks
)
```

### 3. Diagram Understanding
```python
# Understand technical diagrams
response = query_deepseek_vlm(
    prompt="Explain this technical diagram. What process or system does it represent?",
    system="You are a technical documentation expert.",
    image_path="system_diagram.png",
    model="deepseek-vl-7b-chat"
)
```

## Integration with Agent Laboratory Workflow

VLM models can be integrated into any phase of the research workflow:

1. **Literature Review**: Analyze figures and charts from papers
2. **Plan Formulation**: Include visual analysis in research plans
3. **Data Preparation**: Process image datasets
4. **Experimentation**: Run vision-based experiments
5. **Results Analysis**: Interpret visual results
6. **Report Writing**: Include visual analysis in reports

## Supported Image Formats

- PNG (.png)
- JPEG (.jpg, .jpeg)
- WebP (.webp)

## Best Practices

1. **Image Quality**: Use clear, high-resolution images for better results
2. **Prompt Engineering**: Be specific about what you want the model to analyze
3. **Model Selection**: Use 7B for complex analysis, 1.3B for simple tasks
4. **Cost Management**: Monitor token usage and choose appropriate model size
5. **Error Handling**: Implement retry logic for API failures

## Troubleshooting

### Common Issues
1. **Image too large**: Resize images to reasonable dimensions (<2048x2048)
2. **Unsupported format**: Convert to PNG, JPEG, or WebP
3. **API errors**: Check API key and network connectivity
4. **High costs**: Consider using smaller model or optimizing prompts

### Error Messages
- "Image not found": Check file path and permissions
- "Unsupported image format": Convert to supported format
- "API key invalid": Verify DEEPSEEK_API_KEY environment variable
- "Token limit exceeded": Reduce image size or prompt length

## Future Enhancements

Planned improvements for VLM integration:
- Batch image processing
- Image preprocessing utilities
- Advanced cost tracking
- Local model deployment support
- Additional VLM providers

## Contributing

To add support for additional VLM models:
1. Update `inference.py` with new model definitions
2. Add cost mapping in cost estimation functions
3. Update documentation and examples
4. Test with representative image tasks

For questions or issues, please refer to the main Agent Laboratory documentation or open an issue on GitHub.