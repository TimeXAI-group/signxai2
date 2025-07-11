==========
Quickstart
==========

This quickstart guide will help you get up and running with SignXAI2 quickly for both TensorFlow and PyTorch models.

.. contents:: Contents
   :local:
   :depth: 2

Installation
-----------

First, install SignXAI2 with your preferred framework:

.. code-block:: bash

    # For TensorFlow
    pip install signxai2[tensorflow]
    
    # For PyTorch
    pip install signxai2[pytorch]
    
    # For both frameworks
    pip install signxai2[tensorflow,pytorch]

TensorFlow Quickstart
-------------------

Here's a complete example using TensorFlow:

.. code-block:: python

    import numpy as np
    import matplotlib.pyplot as plt
    from tensorflow.keras.applications.vgg16 import VGG16, preprocess_input, decode_predictions
    from tensorflow.keras.preprocessing.image import load_img, img_to_array
    from signxai.tf_signxai import calculate_relevancemap
    
    # Step 1: Load a pre-trained model
    model = VGG16(weights='imagenet')
    
    # Step 2: Remove softmax (critical for explanations)
    model.layers[-1].activation = None
    
    # Step 3: Load and preprocess an image
    img_path = 'path/to/image.jpg'
    img = load_img(img_path, target_size=(224, 224))
    x = img_to_array(img)
    x = np.expand_dims(x, axis=0)
    x = preprocess_input(x)
    
    # Step 4: Get prediction
    preds = model.predict(x)
    top_pred_idx = np.argmax(preds[0])
    print(f"Predicted class: {decode_predictions(preds, top=1)[0][0][1]}")
    
    # Step 5: Calculate explanation with Gradient x Input method
    explanation = calculate_relevancemap('gradient_x_input', x, model, neuron_selection=top_pred_idx)
    
    # Step 6: Normalize and visualize
    abs_max = np.max(np.abs(explanation))
    normalized = explanation / abs_max
    
    plt.figure(figsize=(10, 5))
    plt.subplot(1, 2, 1)
    plt.imshow(img)
    plt.title('Original Image')
    plt.axis('off')
    
    plt.subplot(1, 2, 2)
    plt.imshow(normalized[0].sum(axis=-1), cmap='seismic', clim=(-1, 1))
    plt.title('Explanation')
    plt.axis('off')
    
    plt.tight_layout()
    plt.show()

PyTorch Quickstart
----------------

Here's a complete example using PyTorch:

.. code-block:: python

    import torch
    import numpy as np
    import matplotlib.pyplot as plt
    from PIL import Image
    import torchvision.models as models
    import torchvision.transforms as transforms
    from signxai.torch_signxai import calculate_relevancemap
    from signxai.torch_signxai.utils import remove_softmax
    
    # Step 1: Load a pre-trained model
    model = models.vgg16(pretrained=True)
    model.eval()
    
    # Step 2: Remove softmax
    model_no_softmax = remove_softmax(model)
    
    # Step 3: Load and preprocess an image
    img_path = 'path/to/image.jpg'
    img = Image.open(img_path).convert('RGB')
    
    preprocess = transforms.Compose([
        transforms.Resize((224, 224)),
        transforms.ToTensor(),
        transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
    ])
    
    input_tensor = preprocess(img).unsqueeze(0)  # Add batch dimension
    
    # Step 4: Get prediction
    with torch.no_grad():
        output = model(input_tensor)
    
    # Get the most likely class
    _, predicted_idx = torch.max(output, 1)
    
    # Step 5: Calculate explanation with Gradient x Input method
    explanation = calculate_relevancemap(
        model_no_softmax, 
        input_tensor, 
        method="input_t_gradient",
        target_class=predicted_idx.item()
    )
    
    # Step 6: Normalize and visualize
    # Convert back to numpy for visualization
    abs_max = np.max(np.abs(explanation))
    normalized = explanation / abs_max
    
    # Convert the original image for display
    img_np = np.array(img.resize((224, 224))) / 255.0
    
    plt.figure(figsize=(10, 5))
    plt.subplot(1, 2, 1)
    plt.imshow(img_np)
    plt.title('Original Image')
    plt.axis('off')
    
    plt.subplot(1, 2, 2)
    plt.imshow(normalized[0].sum(axis=0), cmap='seismic', clim=(-1, 1))
    plt.title('Explanation')
    plt.axis('off')
    
    plt.tight_layout()
    plt.show()

Framework-Agnostic Approach
-------------------------

You can also use the framework-agnostic API:

.. code-block:: python

    import signxai
    
    # Will work with either TensorFlow or PyTorch model
    explanation = signxai.calculate_relevancemap(model, input_tensor, method="gradient")
    
    # SignXAI will automatically detect the framework and use the appropriate implementation
    print(f"Available backends: {signxai._AVAILABLE_BACKENDS}")

Multiple Explanation Methods
--------------------------

Compare different explanation methods for the same input:

.. code-block:: python

    # For TensorFlow
    methods = ['gradient', 'gradient_x_input', 'integrated_gradients', 'smoothgrad', 'lrp_z']
    explanations = []
    
    for method in methods:
        explanation = calculate_relevancemap(method, x, model, neuron_selection=top_pred_idx)
        explanations.append(explanation)
    
    # Visualize all methods
    fig, axs = plt.subplots(1, len(methods) + 1, figsize=(15, 4))
    axs[0].imshow(img)
    axs[0].set_title('Original')
    axs[0].axis('off')
    
    for i, (method, expl) in enumerate(zip(methods, explanations)):
        abs_max = np.max(np.abs(expl))
        normalized = expl / abs_max
        axs[i+1].imshow(normalized[0].sum(axis=-1), cmap='seismic', clim=(-1, 1))
        axs[i+1].set_title(method)
        axs[i+1].axis('off')
    
    plt.tight_layout()
    plt.show()

LRP Variants
----------

Layer-wise Relevance Propagation (LRP) has several variants:

.. code-block:: python

    # For TensorFlow
    lrp_methods = [
        'lrp_z',                  # Basic LRP-Z
        'lrpsign_z',              # LRP-Z with SIGN
        'lrp_epsilon_0_1',        # LRP with epsilon=0.1
        'lrp_alpha_1_beta_0'      # LRP with alpha=1, beta=0
    ]
    
    lrp_explanations = []
    for method in lrp_methods:
        explanation = calculate_relevancemap(method, x, model, neuron_selection=top_pred_idx)
        lrp_explanations.append(explanation)
    
    # Visualize LRP variants
    # ...

Creating a Saliency Map
---------------------

Generate a saliency map overlaid on the original image:

.. code-block:: python

    from signxai.common.visualization import normalize_relevance_map, relevance_to_heatmap, overlay_heatmap
    
    # Get explanation
    explanation = calculate_relevancemap(model, input_tensor, method="lrp_epsilon", epsilon=0.1)
    
    # Normalize relevance map
    normalized = normalize_relevance_map(explanation[0].sum(axis=0))
    
    # Convert to heatmap
    heatmap = relevance_to_heatmap(normalized)
    
    # Overlay on original image
    overlaid = overlay_heatmap(img_np, heatmap, alpha=0.7)
    
    plt.figure(figsize=(10, 10))
    plt.imshow(overlaid)
    plt.title('Explanation Heatmap')
    plt.axis('off')
    plt.show()

Next Steps
---------

After this quickstart, you can:

1. Explore different explanation methods in the :doc:`methods_list`
2. Learn about framework-specific features in :doc:`tensorflow` and :doc:`pytorch`
3. Check out complete tutorials in the :doc:`/tutorials/image_classification` and :doc:`/tutorials/time_series`
4. Understand the framework interoperability options in :doc:`framework_interop`