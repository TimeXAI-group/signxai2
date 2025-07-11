SignXAI2 Documentation
=======================

**SignXAI2** is a comprehensive cross-framework explainable AI (XAI) library that provides unified access to explanation methods across TensorFlow and PyTorch. With over 200 XAI methods and automatic framework detection, SignXAI makes it easy to understand and interpret your deep learning models.

=� **Key Features**
-------------------

* **Cross-Framework Compatibility**: Single API works with both TensorFlow and PyTorch
* **200+ XAI Methods**: Comprehensive collection including gradients, LRP, CAM, and more
* **Automatic Framework Detection**: No need to specify framework - SignXAI2 detects it automatically
* **Parameter Mapping**: Consistent parameter names across frameworks
* **Robust Implementation**: Extensive testing and validation across frameworks

<� **Quick Start**
------------------

Install SignXAI2 with your preferred framework:

.. code-block:: bash

    # For TensorFlow only
    pip install signxai2[tensorflow]
    
    # For PyTorch only  
    pip install signxai2[pytorch]
    
    # For both frameworks
    pip install signxai2[all]

Generate your first explanation:

.. code-block:: python

    from signxai2 import explain
    
    # Works with any model - automatic framework detection!
    explanation = explain(model, input_data, 'gradient')

=� **Documentation Structure**
------------------------------

.. toctree::
   :maxdepth: 2
   :caption: Getting Started

   guide/installation
   guide/quickstart
   guide/basic_usage

.. toctree::
   :maxdepth: 2
   :caption: User Guide

   guide/framework_interop
   guide/advanced_usage
   guide/visualization
   guide/tensorflow
   guide/pytorch

.. toctree::
   :maxdepth: 2
   :caption: Tutorials

   tutorials/image_classification
   tutorials/time_series

.. toctree::
   :maxdepth: 2
   :caption: API Reference

   api/unified_api
   api/tensorflow
   api/pytorch
   api/common
   api/utils
   api/methods_list

.. toctree::
   :maxdepth: 1
   :caption: Development

   contributing
   changelog
   license

= **Method Categories**
------------------------

SignXAI2 organizes XAI methods into intuitive categories:

**Gradient-Based Methods**
  * ``gradient`` - Basic gradient attribution
  * ``smoothgrad`` - Noise-averaged gradients for robustness
  * ``integrated_gradients`` - Path integral gradients from baseline
  * ``vargrad`` - Variance-based gradient analysis

**Layer-wise Relevance Propagation (LRP)**
  * ``lrp_epsilon`` - Epsilon rule with stabilization
  * ``lrp_alpha_1_beta_0`` - Alpha-beta rule (�=1, �=0)
  * ``lrp_alpha_2_beta_1`` - Alpha-beta rule (�=2, �=1)
  * ``lrp_z`` - Z+ rule for positive contributions

**Class Activation Methods (CAM)**
  * ``grad_cam`` - Gradient-weighted Class Activation Mapping
  * ``guided_grad_cam`` - Combines GradCAM with guided backprop

**Backpropagation Methods**
  * ``guided_backprop`` - Guided backpropagation
  * ``deconvnet`` - Deconvolutional networks

**Advanced Methods**
  * ``deeplift`` - DeepLIFT attribution
  * ``lime`` - Local interpretable model-agnostic explanations
  * ``shap`` - SHapley Additive exPlanations

=� **Why SignXAI?**
-------------------

**Unified Interface**
    One API works across TensorFlow and PyTorch - no more learning framework-specific tools.

**Extensive Method Collection**
    Over 200 XAI methods implemented with consistent interfaces and validated across frameworks.

**Research-Grade Quality**
    Rigorous implementation following original papers, with cross-framework validation.

**Easy Integration**
    Drop-in replacement for existing XAI workflows with automatic parameter mapping.

**Active Development**
    Regular updates, new methods, and community contributions.

<� **Success Stories**
----------------------

SignXAI is used by researchers and practitioners worldwide for:

* **Medical AI**: Explaining diagnostic model predictions
* **Computer Vision**: Understanding image classification and object detection
* **Time Series Analysis**: Interpreting forecasting and anomaly detection models  
* **NLP**: Analyzing attention patterns and feature importance
* **Research**: Cross-framework method validation and benchmarking

=� **Performance & Reliability**
--------------------------------

* **Cross-Framework Consistency**: Average correlation >0.85 between TensorFlow and PyTorch implementations
* **Comprehensive Testing**: 500+ test cases covering all major method combinations
* **Performance Optimized**: Efficient implementations with minimal overhead
* **Memory Efficient**: Smart batching and memory management for large models

> **Community & Support**
---------------------------

* **GitHub Repository**: https://github.com/signxai/signxai
* **Issue Tracker**: Report bugs and request features
* **Discussions**: Community support and method discussions
* **Contributing**: Guidelines for contributing new methods and improvements

=� **Citation**
----------------

If you use SignXAI in your research, please cite:

.. code-block:: bibtex

    @software{signxai2024,
      title={SignXAI: Cross-Framework Explainable AI Library},
      author={SignXAI Team},
      year={2024},
      url={https://github.com/signxai/signxai}
    }

=� **License**
--------------

SignXAI is released under the MIT License. See the :doc:`license` for details.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`