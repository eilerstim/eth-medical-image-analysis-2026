# ETH Zürich — Medical Image Analysis (2026): Complete Course Notes

> Comprehensive study notes compiled from **every file** in this repository, processed in course order.
> Instructors: **Prof. Ender Konukoglu** (ETH Zürich), **Prof. Mauricio Reyes** (University of Bern), **Dr. Ertunc Erdil** (ETH Zürich).
>
> Each section below corresponds to one source file and captures all the substantive content it contains — topics, definitions, formulas, algorithms, experimental numbers, cited references, and figure descriptions (where inferable). Formulas are rendered in LaTeX notation.

## Source files covered

| # | File | Pages | Topic |
|---|------|-------|-------|
| 1 | `lecture1_introduction.pdf` | 52 | Course introduction: what/why, main tasks, image formats, toolkits |
| 2 | `lecture1_refresher.pdf` | 56 | Math refresher: linear algebra, calculus, probability |
| 3 | `lecture2_enhancement.pdf` | 154 | Image enhancement I: histograms, point operations, filtering |
| 4 | `lecture2_enhancement_contd.pdf` | 100 | Image enhancement II: edges, denoising, advanced filtering |
| 5 | `lecture3_transformations.pdf` | 57 | Spatial transformations I |
| 6 | `lecture4_transformations.pdf` | 134 | Spatial transformations II, interpolation |
| 7 | `lecture5_registration.pdf` | 121 | Image registration |
| 8 | `lecture6_segmentation_introduction.pdf` | 131 | Segmentation introduction: thresholding, clustering, EM |
| 9 | `Lecture_SSM+ASM.pdf` | 69 | Statistical shape models & active shape models |
| 10 | `AtlasPatch_segmentation.pdf` | 52 | Atlas-based & patch-based segmentation |
| 11 | `atlas_patch_segmentation_demo.html` | — | Interactive demo of atlas/patch-based segmentation |
| 12 | `Lecture09_CNN.pdf` | 72 | Convolutional neural networks |
| 13 | `Lecture10_Transformers.pdf` | 51 | Transformers for vision |
| 14 | `Lecture_11-Pixel_level_predictions.pdf` | 140 | Pixel-level prediction networks (segmentation, restoration, registration) |
| 15 | `lecture12_uncertainty_estimation.pdf` | 153 | Uncertainty estimation |
| 16 | `lecture13_domain_adaptation.pdf` | 101 | Domain shift & domain adaptation |
| 17 | `lecture13_semi_supervised.pdf` | 101 | Semi-supervised learning |
| 18 | `Lecture_Interpretability_Reyes_ETHZ_2026.pdf` | 88 | Interpretability & explainable AI |
| 19 | `Example_Exam_with_Answers.pdf` | 5 | Example exam questions with answers |

---
# 1. Lecture 1 — Introduction (`lecture1_introduction.pdf`, 52 slides)

*Ender Konukoglu, ETH Zürich, February 17, 2026*

## 1.1 Motivation (slides 2–6)

The lecture opens with a series of clinical images posing the questions a radiologist implicitly answers — and which medical image analysis (MIA) aims to automate:

- "Why are you taking this course?"
- "Is there something off?" (abnormality detection)
- "Is there a change?" (longitudinal comparison)
- "Morphological change?" (shape/structure change)
- "Is it going worse?" (disease progression)

## 1.2 Lecture outline

1. What and why (medical imaging; what is MIA; why do we need it)
2. Main tasks in medical image analysis (enhancement, segmentation, registration, classification)
3. Image formats (anatomical axes & basic properties; DICOM & volumetric formats)
4. Toolkits (visualization & simple manipulation; processing)
5. Outline for the course

## 1.3 What and why

### Medical imaging

- Medical images show **anatomy and physiology** [image credits: H. Alkhadi, J. Polimeni]. Examples shown: Computed Tomography, CT Angiography, T1-weighted structural MRI, Fractional Anisotropy (diffusion MRI), Apparent Diffusion Coefficient maps, MR Angiography, FDG-PET, PIB-PET.
- An image is **"one of many observations"** — each modality is a different view of the same underlying tissue.
- **Life cycle of a medical image for diagnosis**: acquisition → interpretation → report (illustrated as a workflow diagram).
- **Medical imaging in intervention**:
  - Treatment planning for photon and proton radiotherapy [Unkelbach et al., *Radiotherapy and Oncology* 2018; Unispital Zürich / University of Zürich].
  - MR-guided high-intensity focused ultrasound (HIFU) surgery [Ram et al., *Neurosurgery* 2006;59(5):949–956].

### Imaging modalities ("not a complete list at all")

| Category | Modalities |
|---|---|
| **Structural** (anatomical & microstructural information) | X-ray (2D); Ultrasound (2D & 3D); Computed Tomography, CT (3D); MRI (2D/3D): T1-weighted, T2-weighted, FLAIR, diffusion-weighted, angiography, spectroscopy |
| **Temporal / dynamic** | Ultrasound; dynamic CT; dynamic MRI |
| **Physiologic & metabolic** | Perfusion imaging; flow imaging; functional MRI (fMRI); Positron Emission Tomography (PET); Single-Photon Emission Computed Tomography (SPECT) |
| **Microscopic** | Histology (microscopic images); Optical Coherence Tomography (OCT); micro-CT (µCT) |

Key points: the field is very rich in modalities; they are **different sources of information** — in some cases "different glasses" for the same underlying tissue and physiology.

### What is medical image analysis?

> **Algorithms for automatic interpretation of biomedical images.** The two key words are **algorithmic** and **automation**. [Illustration: www.healthcare.siemens.com]

The main application areas presented:

1. **Detection and localization** — automatic positioning and recognition of structures; useful for visualization, transferring over a network, and search. [Kooi et al., *Medical Image Analysis* 2017 (mammography); Jeon et al., *Academic Radiology* 2013]
2. **Morphological assessment and parcellation** — segmenting entire organs and finer-scale substructures as well as abnormal lesions; provides morphological measurements, quantification for diagnosis and staging, and is the first step in treatment planning.
3. **Image reconstruction and enhancement** — reconstruct images from raw/undersampled data (real image / ground truth vs. observed vs. reconstructed) [Tezcan et al., *IEEE TMI* 2019]; increase SNR [Konukoglu et al., MICCAI 2013]. Goals: remove reconstruction artifacts, accelerate acquisition, improve resolution, improve signal-to-noise ratio (SNR).
4. **Prediction** — predicting diagnosis, risk scores, and treatment outcomes. Examples: cardiac CT with aortic calcification — "would the treatment cause a problem for this person?" [Dan Linh, Hatem Alkhadi, Felix Tanner]; "this person has Mild Cognitive Impairment — will she convert to Alzheimer's Disease, and if so, when?" [ADNI dataset].
5. **Statistical analysis** — population studies comparing healthy controls vs. cases; e.g., cortical-thickness group-difference maps; Alzheimer's disease effects computed from 290 subjects. Goal: identify anatomical/metabolic cross-sectional or longitudinal footprints of conditions.
6. **Many others**: longitudinal analysis ("has the nodule grown between these images? is the change predictive of disease?"), spatio-temporal modeling, extracting deformation fields to understand motion, building mathematical models to describe and simulate physiological phenomena.

### Why do we need medical image analysis?

1. **Throughput** [https://data.oecd.org]: higher number of scans, finer resolution, higher number of modalities → "a lot of images to look at!" → need higher throughput.
2. **Quantitative analysis** — human detection of subtle change is poor. Experiment [Pohl, Konukoglu, et al. 2011]: two tumor images 1 year apart with simulated growth; human accuracy of detection (mean ± std) as a function of true growth:

   | Growth | 1% | 3% | 5% | 11% | 16% | 22% |
   |---|---|---|---|---|---|---|
   | Detection accuracy | 8 ± 8 % | 6 ± 6 % | 28 ± 11 % | 44 ± 9 % | 52 ± 24 % | 88 ± 12 % |

   Motivations: subtle changes for earliest detection; highly accurate measurements; repeatability; changes not visible to the human eye (e.g., texture).
3. **Fusion of information** — different modalities have different physical and metabolic characteristics; combining information from different images; image → numerical matrix; fusion with other quantitative sources (labs, genetics, …).

## 1.4 Main tasks in medical image analysis

Common formal setting: image domain $x \in \Omega \subset \mathbb{R}^d$, $d = 2$ or $3$; per-pixel feature vector $f(x) = [I(x),\, I(N(x)),\, J(N(x)), \dots]$ — intensity at the pixel, in its neighborhood $N(x)$, and in other modalities.

### Enhancement

- **Principle**: at each pixel/voxel assign a new value or set of values.
- The new value can represent: noise-free intensities; higher-resolution information (e.g., predict values of pixels with half the pixel size); artifact-free intensities (motion, streak artifacts); **synthesis** — intensity of a different modality (e.g., CT intensity from MRI).
- Formalism: output intensities $J(x) \in \mathbb{R}^D$; enhancement algorithm $G$ approximates $J(x) \approx G(f(x))$.

### Segmentation

- **Principle**: at each pixel/voxel assign a label $L(x) \in \{0, \dots, N\}$, approximated by $L(x) \approx S(f(x))$.
- Labels can be: organs (liver, spine, …), parts of organs (individual vertebrae, …), lesions (tumors, pathologies, …).
- **Techniques overview** (taxonomy given on the slide):
  - *Thresholding and histogram-based*
  - *Clustering*: K-means, unsupervised learning, non-parametric modeling, …
  - *Graph partitioning*: watershed, graph-cuts, random walker, minimum spanning forest, …
  - *Region growing*
  - *Variational and PDE-based*: Chan–Vese model, Mumford–Shah model, active shape models, level sets, fast marching, …
  - *Discriminative modeling*: random forests, conditional random fields, supervised convolutional neural networks, …
  - *Generative modeling*: expectation-maximization, Markov random fields, atlas-based segmentation, variational inference, …

### Registration

- **Principle**: determine the spatial transformation between two images (illustrated with Moving / Target / Aligned / Grid Deformed panels).
- Formalism: images $I(x), J(x)$; spatial transformation $T(x): \Omega \to \mathbb{R}^d$ such that $J(T(x)) = I(x)$ — creates a **correspondence between points**.
- $T(\cdot)$ can be linear or non-linear. The problem is often **ill-posed**, so **regularization** $R(T(x))$ is used to impose physical constraints.
- **Taxonomies**:
  - *Problem-based*: intra-subject vs. inter-subject; unimodal vs. multimodal.
  - *Transformation-based*: **linear** (rigid, similarity, affine) vs. **non-linear/deformable**: landmark-based registration; linear basis function models (B-spline registration, RBF registration, …); dense field models (demons, diffeomorphic demons, Large Deformation Diffeomorphic Metric Mapping (LDDMM), poly-affine, …); discrete optimization (DROP, …); feature-based (HAMMER, …).

### Classification / Regression

- **Principle**: mapping from image to label.
  - Classification: $F: \Omega_c \subseteq \Omega \to \{0, \dots, N\}$
  - Regression: $F: \Omega_c \subseteq \Omega \to \mathbb{R}^N$
- Labels: diagnosis (disease/healthy), outcome prediction (success/complication), presence of an object.
- $\Omega_c$ can be the entire image ($F(I)$: diagnosis, outcome prediction) or part of an image ($F(I_c)$: localization).
- Segmentation and registration can be viewed as classification/regression with special structure — per-pixel/voxel prediction.
- **Techniques**: feature extraction (morphological descriptors, texture features, SIFT, HOG, multimodal features) + supervised learning: linear regression (both prediction and statistical analysis), support vector machines and kernel machines, random forests, AdaBoost, neural networks.

## 1.5 Image formats

### Anatomical axes and basic properties

- **Anatomical axes** and the standard **cross-sections of a volumetric image** (axial/transverse, sagittal, coronal) [NIH illustration].
- **Basic image properties (volume)**:
  - **Image size** — number of pixels/voxels; 2 or 3 integers; e.g., 256×256×128.
  - **Pixel/voxel size** — real-world size a pixel/voxel occupies; 2–3 real numbers; e.g., 0.6×0.6×3.5 mm³. *Sometimes called "resolution," but pixel size and resolution are different things for acquisition people.*
  - **Field of view (FOV)** — real-world size of the entire image = image size × pixel size; e.g., 153.6×153.6×448 mm³.
- **Beyond**:
  - *Temporal information*: additional image dimension (a volumetric time series is 4-D); temporal resolution matters in fMRI, perfusion, dynamic cardiac imaging, …
  - *Metabolic information*: additional dimension holding number of metabolites (MR spectroscopy).
  - *Transformations*: transformation matrices between **voxel coordinates and real-world coordinates** (patient position on the table) [see https://www.slicer.org/wiki/File:Coordinate_sytems.png].

### DICOM and volumetric formats

- **DICOM** (Digital Imaging and Communications in Medicine): keeps the image plus all basic image properties, and additional information — scanner, scan type, ordering/conducting physician, patient information, … It is the **standard for storing and transmitting data**, the most commonly used format in practice (all vendors, all centers, all doctors; used in clinical practice), but turns out to be **difficult to manage for processing**.
- **Volumetric research formats**: Analyze (Mayo Clinic); **NIfTI** (Neuroimaging Informatics Technology Initiative — replacement for Analyze, https://nifti.nimh.nih.gov/nifti-1/); **NRRD** ("nearly raw raster data"). Which to use is a matter of preference — useful for engineering, but you must handle DICOM to work with clinicians.

## 1.6 Toolkits

- **Visualization & simple manipulation**: ITK-SNAP (great basic visualization of volumetric data + simple segmentation routines, http://www.itksnap.org); OsiriX (popular among clinicians); Freeview (from FreeSurfer); MedINRIA; Mango; ParaView (scientific visualization, great for meshes).
- **Processing libraries**: ITK (Insight Toolkit), VTK (Visualization Toolkit), TensorFlow, PyTorch, scikit-learn, NiBabel (read medical volumes in Python), Iso2Mesh (mesh generation from volumetric data), GitHub resources.
- **Software suites** (collections of executables): FreeSurfer, FSL, Camino, SPM.
- **Visualization + processing with GUI**: 3D Slicer (https://www.slicer.org/).

## 1.7 Outline for the course

1. **Enhancement and pre-processing**
2. **Image registration**: spatial mappings and interpolation; transformation models; registration
3. **Segmentation**: unsupervised segmentation/clustering; atlas- and patch-based segmentation; statistical shape models; active shape models
4. **Deep learning models**: convolutional neural networks; transformers; pixel-wise prediction models (segmentation, restoration, registration); domain adaptation and learning from unlabeled data; uncertainty estimation; interpretable and explainable models

---
# 2. Lecture 1 — Refresher (`lecture1_refresher.pdf`, 56 slides)

*Ender Konukoglu, ETH Zürich, February 17, 2026 — "Just pointers and reminders"*

**Outline**: basic notation · probabilistic modeling · optimization, cost function and regularization · linear basis models and function parameterizations · spatial transformations · derivative approximations.

## 2.1 Basic notation

- **Continuous version**: a volumetric grayscale image is a function $I: \Omega \to \mathbb{R}$, where $\Omega \subset \mathbb{R}^3$ is the image domain; $I(x)$ is the intensity at point $x \in \Omega$, $x = [x_1, x_2, x_3]$.
- **Discrete version**: $\Omega \subset \mathbb{Z}^3$ is the discrete image domain (Cartesian grid); $I(x)$ is the intensity at $x = [i, j, k]$.
- **Multiparametric images** (diffusion-weighted MRI, spectroscopy, or several modalities stacked): $I: \Omega \to \mathbb{R}^N$; $I(x)$ is the **vector** of intensities at $x$.
- **Dynamic images** (temporal information): $I: \Omega \times \mathbb{R}^+ \to \mathbb{R}^N$; $I(x, t)$ is the intensity at point $x$ and time $t$.

## 2.2 Probabilistic modeling

### PDF, CDF, PMF

- The intensity at each point $I(x) \in \mathbb{R}$ is treated as a **random variable**. (Other functions can be random variables too, e.g. the transformation $T(x)$ or discrete labels $L(x)$.)
- Continuous random variable (dropping $(x)$ for simplicity):
  - $p(i)$ — **probability density function (PDF)**
  - $P(i) = \Pr[I \le i] = \int_{-\infty}^{i} p(j)\,dj$ — **cumulative distribution function (CDF)**
  - (Slide shows an example Gaussian-like PDF over $I \in [-6, 6]$ with peak ≈ 0.40 and its sigmoid-shaped CDF from 0 to 1.)
- Discrete random variable $L$: $p(l) = p(L = l)$ — **probability mass function (PMF)**. A PMF can be seen as a PDF and is often just called a PDF in scientific articles.

### Histogram of an image

If each pixel intensity is considered an **independent realization** of the random variable $I$, then the (normalized) **histogram is an approximation to the PDF** and the **cumulative histogram approximates the CDF**. (Slide: brain MR image, its histogram over intensities 0–300, and the cumulative histogram.)

### Conditionals and Bayes' rule

With two variables $I$ (intensity) and $L$ (label):

- $p(i, l)$ — joint distribution
- $p(i) = \sum_{l=0}^{N} p(i, l)$ and $p(l) = \int_{-\infty}^{\infty} p(i, l)\,di$ — marginal distributions
- $p(i \mid l)$ and $p(l \mid i)$ — conditional distributions
- Product rule: $p(i, l) = p(i \mid l)\,p(l) = p(l \mid i)\,p(i)$
- **Bayes' rule** links the conditionals:

$$p(i \mid l) = \frac{p(l \mid i)\,p(i)}{p(l)} \qquad p(l \mid i) = \frac{p(i \mid l)\,p(l)}{p(i)}$$

- In a large variety of problems one variable is observed and the other is not. If $i$ is observed: $p(i \mid l)$ is the **likelihood**, $p(l)$ the **prior distribution**, $p(l \mid i)$ the **posterior distribution**.

### Posterior distribution, MAP and MLE

- A large range of problems can be formulated as: *given an observation $i$, estimate $l$*. Example problems: image enhancement, segmentation, and even registration.
- The generic solution is to determine the posterior — **Bayesian inference**: $p(l \mid i) = p(i \mid l)p(l)/p(i)$.
- Computing $p(i)$ requires summing (or integrating) over **all** $l$, which can be infeasible. Alternative: find the $l$ maximizing the posterior — the **Maximum-A-Posteriori (MAP)** estimate:

$$\arg\max_l p(l \mid i) = \arg\max_l p(i \mid l)\,p(l) = \arg\max_l \big[\log p(i \mid l) + \log p(l)\big]$$

- Posterior/MAP require a prior. When no prior exists, use the **Maximum Likelihood Estimate (MLE)**: $\arg\max_l p(i \mid l)$.
- **MLE = MAP when the prior is uniform**, i.e. $p(l) = c\ \forall l$.

## 2.3 Cost function, regularization and optimization

### Data term and regularization

Most problems in medical image analysis are formulated as optimization problems:

$$\arg\min_\theta \underbrace{\mathcal{L}(\theta)}_{\text{cost function}} = \underbrace{D(I; \theta)}_{\text{data term}} + \lambda \underbrace{R(\theta)}_{\text{regularization}}$$

or the related form with explicit constraints: $\arg\min_\theta D(I; \theta)$ such that $R(\theta) = 0$.

**Examples:**

- *Image denoising* — $I$ noisy, retrieve denoised $J$ (total-variation style):

$$\arg\min_J \|I - J\|_2^2 + \lambda \|\nabla J(x)\|_1 = \arg\min_J \int (I(x) - J(x))^2 dx + \lambda \int |\nabla J(x)|\,dx$$

- *Image registration* — determine $T$ between $I$ and $J$ (with a linear-elasticity-type regularizer):

$$\arg\min_T \int (I(x) - J(T(x)))^2 dx + \int \|\alpha \Delta T(x) + \beta \nabla(\nabla \cdot T(x)) - \gamma T(x)\|_2^2\, dx$$

- The **MAP estimate has the same form**: $\arg\max_\theta \log p(i\mid\theta) + \log p(\theta) = \arg\min_\theta -\log p(i\mid\theta) - \log p(\theta)$. Regularizers can be thought of as priors with $-\log p(\theta) \propto R(\theta)$.

### Optimization

Basic quantities for discrete $\theta$:

- **Gradient**: $\nabla \mathcal{L}(\theta) = \left[\frac{\partial \mathcal{L}}{\partial \theta_1}, \dots, \frac{\partial \mathcal{L}}{\partial \theta_d}\right]^T$
- **Hessian**: $H_{jk} = \frac{\partial^2 \mathcal{L}(\theta)}{\partial \theta_j \partial \theta_k}$ (full $d \times d$ matrix of second partial derivatives)

Algorithm families:

- **Gradient-based**: gradient descent/ascent, Newton's method, limited-memory BFGS (L-BFGS), …
- **Gradient-free** (because sometimes the gradient is difficult to evaluate): Nelder–Mead simplex, simulated annealing, Powell's method, BOBYQA, …

References given: Boyd & Vandenberghe, *Convex Optimization*, Cambridge University Press, 2004; Bertsekas, *Nonlinear Programming*, Athena Scientific, 2016.

### Calculus of variations

When $\theta$ is **continuous** (a deformation field, the denoised version of an image, …), the cost function is called a **functional**:

$$\mathcal{L}(\theta) = \int_{x_a}^{x_b} L(x, \theta, \nabla\theta)\, dx$$

- The analogue of the gradient is the **first variation**:

$$\delta \mathcal{L}(\theta) \triangleq \lim_{\epsilon \to 0} \frac{\mathcal{L}(\theta + \epsilon\eta) - \mathcal{L}(\theta)}{\epsilon}$$

for arbitrary $\eta$ vanishing at the boundaries.

- Setting the first variation to 0 for all $\eta$ gives the **Euler–Lagrange equation**:

$$\frac{\partial L}{\partial \theta} - \sum_{k=1}^{d} \frac{d}{dx_k} \frac{\partial L}{\partial \theta^{(k)}} = 0, \qquad \theta^{(k)} = \frac{\partial \theta}{\partial x_k}$$

- The left-hand side is the **functional derivative** $\delta \mathcal{L}/\delta\theta$, often used in gradient descent/ascent.
- Reference: Bruce van Brunt, *The Calculus of Variations*, Springer, 2004.

## 2.4 Linear basis models and function parameterizations

### Basics

- Many problems require **optimizing over a function** (non-linear registration → transformations; bias correction → bias field). Discretizing the Euler–Lagrange equation at every point is an option, but something with **fewer parameters** is very useful → function parameterization for optimization.
- In finite vector spaces: $\vec{v} = a_1 \vec{b}_1 + a_2 \vec{b}_2 + \cdots + a_d \vec{b}_d = B\vec{a}$, where $\vec{b}_i$ are basis functions (columns of $B$) and $a_i$ coefficients.
  - If the $\vec{b}_i$ are **orthogonal** ($\vec{b}_i^T \vec{b}_j = 0\ \forall i \ne j$): $a_i = \vec{v}^T \vec{b}_i / \|\vec{b}_i\|^2$.
  - Otherwise **ordinary least squares (OLS)**: $\arg\min_{\vec a} \|\vec v - B \vec a\|_2^2 = (B^T B)^{-1} B^T \vec v$.
  - For known $\vec b_i$, $\vec a$ is a parameterization of $\vec v$.

### Function parameterizations with linear basis models (global)

$$f(x) = \sum_i^d a_i b_i(x)$$

with smooth basis functions $b_i(x)$; the parameterization is $\vec a$. To determine $\vec a$, the space is discretized and a $B$ matrix formed. **This parameterization cannot represent all functions.** Examples: **polynomials** — bias-field correction in MRI; **splines** — non-linear registration.

### Kernel-based parameterization

$$f(x) = \sum_{i=1}^d a_i K(x, x_i)$$

where $x_i$ are **control points** and $K(x, x_i)$ is a kernel function. **Radial basis functions (RBF)** are often used: $K(x, x_i) = K(\|x - x_i\|_2)$. Popular choices:

- **Gaussian**: $K(\|x - x_i\|_2) = \exp\left(-\frac{\|x - x_i\|_2^2}{\sigma^2}\right)$
- **Thin-plate spline**: $K(\|x - x_i\|_2) = \|x - x_i\|_2^2 \ln(\|x - x_i\|_2)$

Examples: landmark-based registration, linear/non-linear registration, kernel density estimation.

## 2.5 Spatial transformations

### Linear transformations

$T(\vec x) = \mathbf{T}\vec x$ — used in linear image registration.

| Type | 2D dof | 3D dof | Use |
|---|---|---|---|
| **Rigid** | 3 (2 translation + 1 rotation) | 6 (3 translation + 3 rotation) | intra-subject registration, multi-modal intra-subject registration |
| **Similarity** | 4 (rigid + scale) | 7 (rigid + scale) | coarse inter-subject alignment; initialization for non-linear |
| **Affine** | 6 | 12 | often not used in full dof; a 9-dof version (3 translation + 3 rotation + 3 scale) may be preferred |

Decomposition written on the slide (scale × rotations + translation):

$$\mathbf{T} = \underbrace{\begin{bmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & s_z \end{bmatrix}}_{\text{scale}} \underbrace{\begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos\theta & -\sin\theta \\ 0 & \sin\theta & \cos\theta \end{bmatrix}}_{\text{rotation around }x} \underbrace{\begin{bmatrix} \cos\phi & 0 & \sin\phi \\ 0 & 1 & 0 \\ -\sin\phi & 0 & \cos\phi \end{bmatrix}}_{\text{rotation around }y} \underbrace{\begin{bmatrix} \cos\gamma & \sin\gamma & 0 \\ -\sin\gamma & \cos\gamma & 0 \\ 0 & 0 & 1 \end{bmatrix}}_{\text{rotation around }z} + \underbrace{\begin{bmatrix} t_x \\ t_y \\ t_z \end{bmatrix}}_{\text{translation}}$$

*(Note: the slide prints $\cos(\theta)$ in the bottom-right entry of the $y$-rotation matrix — a typo for $\cos(\phi)$.)*

### Non-linear transformations

$$T(\vec x) = \vec x + \underbrace{\vec u(\vec x)}_{\text{displacement field}}$$

Used in non-linear registration. Models:

- **Linear basis function models** — additive displacement fields.
- **More advanced techniques based on composition of transformations**: $T(\vec x) = T_n \circ T_{n-1} \circ \cdots \circ T_1(\vec x)$ where $\circ$ is function composition — allows modeling **diffeomorphisms** (see *Computational Anatomy*).

### Transformation-related identities

For $T(\vec x) = [T_1(\vec x), T_2(\vec x), T_3(\vec x)]^T$:

- **Jacobian matrix**: $J = \left[\frac{\partial T_m}{\partial x_n}\right]_{3\times 3}$ (all partial derivatives of components w.r.t. coordinates).
- **Jacobian determinant** quantifies **local volumetric change**: $\det(J) = 1$ → no change; $\det(J) < 1$ → compression; $\det(J) > 1$ → expansion.
- **Divergence** $\nabla \cdot T$ = trace of the Jacobian — also informs about the amount of compression/expansion.
- **Curl** $\nabla \times T$ — information on the amount of infinitesimal rotation.

## 2.6 Derivative approximations

**Finite difference** approximations are used most often. At grid point $x_0$ with grid spacing $\Delta x$:

- First-order derivative (forward / backward / central):

$$\frac{df}{dx}\Big|_{x_0} \approx \frac{f(x_0 + \Delta x) - f(x_0)}{\Delta x} \approx \frac{f(x_0) - f(x_0 - \Delta x)}{\Delta x} \approx \frac{f(x_0 + \Delta x) - f(x_0 - \Delta x)}{2\Delta x}$$

- Second-order derivative:

$$\frac{d^2 f}{dx^2}\Big|_{x_0} \approx \frac{f(x_0 + \Delta x) - 2f(x_0) + f(x_0 - \Delta x)}{\Delta x^2}$$

Many different approximations exist.

---
# 3. Lecture 2 — Image Enhancement and Preprocessing, Part I (`lecture2_enhancement.pdf`, 154 slides)

*Ender Konukoglu, ETH Zürich, February 17–24, 2026*

## 3.1 Introduction

- Pipeline position: **Acquisition → (Reconstruction, Enhancement, Pre-processing) → Segmentation / Registration / Classification–Prediction–Detection / Population analysis**. Enhancement and pre-processing form the **initial steps for any type of analysis** — the same applies to methods using machine learning, probabilistic, variational or energy-based formulations.
- **Conditions to enhance** (shown on an example image): bad contrast, varying intensity statistics, noise, bias field, variation in pixel size.
- **Why enhancement?** Simplifying interpretation; better visualization; normalization for further processing.
- Deck outline: intensity normalization · noise suppression · exercise/challenge · bias correction · variations in image and pixel size · additional material (contrast enhancement, linear filters).

## 3.2 Intensity normalization

### Problem source

- **Intensity variations between images** arise from differences in scanners, protocols, acquisition software, reconstruction method, …
- Particularly important for **MRI due to lack of absolute intensities**; less of a problem for modalities with absolute intensities (e.g., **Hounsfield Units in CT**), though variations still occur.
- Intensity variations cause adverse effects for subsequent processing steps; **particularly problematic for machine learning** — intensity differences between training and test samples constitute **domain shift** ("think of images at night and day"). **Remains an open problem.**

### The ideal

Whenever (a) two images are compared (e.g., registration), (b) two groups are compared (e.g., statistical analysis), or (c) a model is trained on one set of images and applied to another (training/testing), you would like the images to have the same intensity **"characteristics"** — meaning the intensity distribution, including global and local intensity and contrast.

### Approach I — Min-max normalization

$$f(j) = \frac{j - j_{\min}}{j_{\max} - j_{\min}} (i_{\max} - i_{\min}) + i_{\min}$$

where $i$ and $j$ are the intensities of the two images (matching the range of $j$'s histogram to $i$'s).

- Min/max values are **sensitive to outliers** → often the intensities at the $\epsilon\%$ and $(1-\epsilon)\%$ points of the CDF are used instead (e.g., $\epsilon = 2$).
- Commonly used as a preprocessing step in machine-learning methods.
- **What it solves**: global intensity shifts and multiplicative factors — yes. **Contrast differences — no.**

### Approach II — Matching mean and standard deviation

$$f(j) = (j - \mu_j)\frac{\sigma_i}{\sigma_j} + \mu_i, \qquad \mu_i = \frac{1}{N}\sum_{x=1}^N i(x), \qquad \sigma_i^2 = \frac{1}{N-1}\sum_{x=1}^N (i(x) - \mu_i)^2$$

- Matches second-order statistics; **less sensitive to outliers**, but robust statistics (median, robust variance) may be used for even better behavior.
- Often used in machine-learning methods.
- Solves global shifts and multiplicative factors — yes. **Contrast differences — no.**

### Quiz slide

- *Both min-max and mean-std matching are linear mappings — True or False?* **True** (linear except for the bias term; linear in homogeneous coordinates).
- *Can these transformations change the contrast of an image?* **No.** → We need **non-linear transformations in the intensity space**.

### Approach III — Histogram normalization with landmarks (piecewise linear)

Example with quartiles + min/max as landmarks $(j_{beg}, j_1, j_2, j_3, j_{end}) \to (i_{beg}, i_1, i_2, i_3, i_{end})$; piecewise linear mapping:

$$f(j) = \begin{cases} (j - j_{beg})\frac{i_1 - i_{beg}}{j_1 - j_{beg}} + i_{beg} & j \le j_1 \\ (j - j_1)\frac{i_2 - i_1}{j_2 - j_1} + i_1 & j_1 < j \le j_2 \\ (j - j_2)\frac{i_3 - i_2}{j_3 - j_2} + i_2 & j_2 < j \le j_3 \\ (j - j_3)\frac{i_{end} - i_3}{j_{end} - j_3} + i_3 & j_3 < j \end{cases}$$

**Nyul's method** [Nyul, Udupa and Zhang, *IEEE TMI* 2000]:

- Piecewise linear map matching **landmarks of image histograms**; overall a **non-linear** mapping.
- Possible landmarks: min and max; mean or median; quartiles, deciles; modes, …
- Less sensitive to outliers; **widely used**.
- **Population-wide use**: estimate landmark positions from population data as averages; map a new image's landmarks to those averages.
- *Why did the contrast change after matching?* Due to the **non-linearity** of the mapping.
- **"Brilliance" of Nyul's method**: matching only the *locations* of the landmarks does **not** make the histograms perfectly equal — it aligns the distributions while preserving image content.

### Analysis / caveats

- **Image content**: histogram matching does not know about image content; all pixels are considered independent; no spatial information/correlation between neighboring pixels is used. Matching **gradients** has been proposed as a remedy.
- **Object size**: differences in object size are a problem — a perfect histogram match would *not* be preferred then; use **mode matching** if object sizes differ.
- **Pathologies**: presence of pathologies (e.g., large brain tumors vs. healthy brains) changes histograms; matching pathology-bearing images is hard; robust statistics and outlier detection can help.

## 3.3 Noise suppression

### Problem

- Sources: **random noise** and **imaging artifacts** — imperfections in the acquisition device, artifacts due to fast acquisition, artifacts due to implants, …
- Formalization (clean image $I$, noisy image $J$; task: given $J$, remove noise to get $I$):
  - **Additive noise**: $J(x) = I(x) + \epsilon(x)$; $\epsilon(x)$ often assumed i.i.d.; e.g., Gaussian noise $\epsilon \sim \mathcal{N}(0, \sigma)$.
  - **Multiplicative noise**: $J(x) = I(x)\,\epsilon(x)$; e.g., **speckle** in ultrasound and optical coherence tomography.
  - **More complicated**: noise in the MR **magnitude** image, $J(x) = \sqrt{(I_r(x) + \epsilon)^2 + (I_i(x) + \eta)^2}$ with $\epsilon, \eta \sim \mathcal{N}(0, \sigma)$ → **Rician noise** [Gudbjartsson and Patz, *MRM* 1995].
- **Methods overview** — generic: convolutional/linear, median filtering, anisotropic diffusion, non-local means, optimization-based. Specialized methods for specific applications/noise models are not studied in the course.

### Linear (convolutional) filters

Convolution $\tilde I = J * S$ with structural element (kernel) $S$:

$$\tilde I(x,y,z) = \iiint J(x-r,\, y-p,\, z-q)\, S(r,p,q)\, dr\, dp\, dq \qquad \tilde I(i,j,k) = \sum_r \sum_p \sum_q J(i-r, j-p, k-q)\, S(r,p,q)$$

**Analysis** (comparison figure: mean $S_7$, binomial $S_7$, Gaussian $\sigma = 2$): extremely efficient to apply; easy to implement; **blurs edges**; not context aware; sensitive to outliers.

> **Remark (key point):** It is **not possible to remove noise and not blur edges at the same time with a linear filter**. You need non-linear filtering for this.

### Median filter

- For each location, **rank-order all neighboring intensities** and assign the **median** as the result. Worked example on the slide: values 1, 2, 2, 3, 2, 2, 9, 3 → sorted 1, 2, 2, 2, 2, 3, 3, 9 → median **2**.
- Sliding-window operation similar to convolution. Other **rank-order filters** (min, max) are defined similarly. These are **non-linear filters**; two rank filters are **not commutative**: $\min(\mathrm{median}(I)) \ne \mathrm{median}(\min(I))$.
- **Properties**: robust to outliers — does not respond to spikes; preserves discontinuities such as edges.
- **Analysis**: robustness and edge preservation; easy and efficient implementation; **does not introduce new intensities** — consequences: (i) keeps intensities integer, (ii) applying median filtering to *label maps* as a post-processing step will not introduce new labels and removes islands (useful after segmentation). Downsides: **patchy** results; image content can change (structures may be added or removed).

### Non-local means (NLM)

[Buades, Coll and Morel, CVPR 2005] — very widely used, very competitive results; an **extension of bilateral filtering**.

- Most noise-suppression methods can be written as a weighted average:

$$I(x) = \sum_{y \in \Omega} w(x, y) J(y), \qquad 0 \le w(x,y) \le 1, \quad \sum_{y\in\Omega} w(x,y) = 1\ \forall x$$

(mean and Gaussian filters are special cases with particular weights).

- **NLM weight** — similarity between the *image patches* around $x$ and $y$:

$$w(x, y) = \frac{1}{Z(x)} \exp\left(-\frac{\|J(N(x)) - J(N(y))\|_2^2}{h^2}\right)$$

with normalization constant $Z(x)$, degree of filtering $h$, and neighborhood (patch) $N(x)$.

- **Intuition** (slide embeds the relevant page of Buades et al. 2005; summary): earlier *neighborhood filters* — Yaroslavsky (1985), SUSAN (1995), bilateral (1998) — average pixels whose single grey values are similar (optionally also weighting spatial distance); they avoid blurring edges when the inter-region grey difference exceeds $h$, but comparing single noisy pixel values is not robust and creates artificial "shock" artifacts. NL-means instead compares the **whole geometric configuration of a patch**: the weight decays with the (Gaussian-weighted) Euclidean distance between patch vectors. Under noise, $\mathbb{E}\|v(N_i) - v(N_j)\|^2_{2,a} = \|u(N_i) - u(N_j)\|^2_{2,a} + 2\sigma^2$ — the expected distance preserves the *ordering* of similarity, which makes the method robust. A pixel with the same grey value but a different neighborhood gets weight ≈ 0.
- **Key ideas**: while smoothing point $x$, use areas with **similar image content**; if noise is random, averaging similar areas removes it; exploits **redundancy** in images — local structures repeat.
- **Practical properties**:
  - Increasing $h$ → more smoothing (examples with $h = 5, 15, 50$).
  - Model parameters: $h$ (depends on the noise level), size of patch $N(x)$, size of search window $W(x)$.
  - Averaging is usually restricted to a small **search window**: $I(x) = \sum_{y \in W(x)} w(x,y) J(y)$.
  - Size of $N(x)$ matters: $N(x) = \Omega$ → mean filtering; $N(x) = \{x\}$ → Yaroslavsky's filter, a special case of bilateral filtering.
- **Analysis**: higher noise suppression; better edge preservation; may create artifacts; important to **estimate the noise level** — different algorithms exist [Coupé et al., *Medical Image Analysis* 2010]. Extensions to MRI and diffusion MRI [Manjón et al., *Medical Image Analysis* 2008; Wiest-Daessle et al., MICCAI 2008]. **Currently the state-of-the-art filtering technique that is not based on machine learning.**

### Comparison of the three filter families

| Convolutional filters | Median filters | Non-local means |
|---|---|---|
| Efficient to apply, easy to implement | Efficient to apply, easy to implement | Higher noise suppression |
| Linear → easy to analyze mathematically | Robust to outliers | Better edge preservation |
| Blurs edges | Edge preservation | May create artifacts |
| Not context aware | No new intensities introduced | Not as efficient to apply as the others |
| Sensitive to outliers | Patchy results; image content may change | |

## 3.4 Summary and exercise

- Summary figure comparing: histogram equalization, intensity normalization, linear filtering, median filtering, non-local means, and **TV (total variation) denoising with $\lambda = 0.3$**.
- **Noise suppression challenge**: high-resolution MRI is hard to acquire; one way is to acquire many images of the same subject and average them — ~8 images yield good SNR but lead to long acquisitions. *Can you reduce this time with noise suppression?* (Single acquisition vs. average-of-8 shown; details on the Moodle platform.)
- Next week: bias correction; variations in image and pixel size; discussion of the exercise.

## 3.5 Additional material A — Contrast enhancement

- **Problem**: under-"exposure" or over-"exposure" → enhance the contrast of the observation.
- **Histogram view**: in a nice image, intensities are well spread across the range; in a bad-contrast image, the interesting parts are **squeezed into a small intensity range**. Goal: redistribute intensities over a larger range.
- **Approach — flatten the histogram** to cover as much of the intensity region as possible (flat histogram ↔ linear CDF).
- **Histogram equalization principle**: find a **monotonically increasing** intensity mapping ($\forall i \in [0, i_{\max}],\ i \to f(i)$ with $i \ge j \Rightarrow f(i) \ge f(j)$) that flattens the histogram. In the continuous domain the mapping is the **scaled CDF**:

$$f(i) = i_{\max} P(i) = i_{\max} \int_0^i p(j)\, dj$$

- Proof that it flattens the distribution (change of variables):

$$p(f(i)) = p(i) \frac{di}{df(i)} = p(i)\cdot\frac{1}{p(i)}\cdot\frac{1}{i_{\max}} = \frac{1}{i_{\max}}$$

- **Algorithmically** (quantized image, discrete histogram): $f(i) = i_{\max} \sum_{j=0}^{i} h_I(j)$, where $i_{\max}$ is the maximum intensity in the original image and $h_I(j)$ the (normalized) histogram value at intensity $j$.
- **Question**: histogram equalization improves contrast for under-/over-exposed images — *can it improve the image if the intensities of two different tissue types overlap?* (No — a monotone intensity-only mapping cannot separate overlapping intensity populations.)

## 3.6 Additional material B — Linear filters in detail

Convolution definition and a graphical sliding-kernel illustration, then the three standard smoothing kernels:

- **Mean filtering**: average in a neighborhood; $S_3 = \frac{1}{9}\begin{bmatrix}1&1&1\\1&1&1\\1&1&1\end{bmatrix}$; larger kernels ($S_5$, $S_7$) similar and smooth more; **separable** → very efficient: $S_3 = \frac{1}{9}[1,1,1] * [1,1,1]^T$.
- **Binomial filtering**: iterative convolutions of $[1,1]$; integer filters; $S_3 = \frac{1}{16}\begin{bmatrix}1&2&1\\2&4&2\\1&2&1\end{bmatrix}$; larger kernels by successive convolutions of smaller ones; smooth more; separable: $S_3 = \frac{1}{16}[1,2,1] * [1,2,1]^T$; **better properties than mean filtering in the frequency domain** (not covered further).
- **Gaussian filtering**: kernel constructed from a Gaussian centered on the structural element, e.g. $S_3(i,j) = \frac{1}{C}\exp\left(-\frac{(i-1)^2 + (j-1)^2}{2\sigma^2}\right)$ with normalizing constant $C$; the Gaussian has **infinite support**, so a finite structural element only approximates it; the element size depends on $\sigma$ (larger $\sigma$ needs a larger element for a good approximation); larger $\sigma$ smooths more. Examples: $\sigma = 1, 1.5, 2$.

---
# 4. Lecture 2 — Image Enhancement and Preprocessing, Part II (`lecture2_enhancement_contd.pdf`, 100 slides)

*Ender Konukoglu, ETH Zürich, February 24 – March 3, 2026*

Recap of the introduction (same pipeline and "conditions to enhance" as Part I), then three main topics: **energy-based noise suppression**, **bias correction (N3)**, and **variations in image and pixel size**.

## 4.1 Noise suppression — energy-based / optimization-based methods

- Starting from the additive model: $\underbrace{J(x)}_{\text{observed}} = \underbrace{I(x)}_{\text{desired}} + \underbrace{\epsilon(x)}_{\text{noise}}$.
- **Denoising as an energy minimization problem**:

$$\hat I = \arg\min_I\ \lambda D(I, J) + R(I)$$

  - $D(I, J)$: **data consistency** term
  - $R(I)$: **regularization** — embedding prior information ("opinions") about the desired image
  - $\lambda$: weighting coefficient
  - Alternative constrained form: $\hat I = \arg\min_I R(I)$ such that $D(I, J) = 0$.

### Total variation (TV) denoising

[Rudin, Osher and Fatemi, *Physica D*, 1992]

$$D(I, J) = \int (I(x) - J(x))^2\, dx = \|I - J\|_2^2 \qquad R(I) = \int \|\nabla I\|\, dx = \|I\|_{TV}$$

- $\lambda$ encodes the allowed noise level in the observation, with $\lambda = 1/2\sigma^2$.
- TV aims for an image with **minimal variation** (piecewise-constant tendency). Experiments shown for $\lambda = 5.0,\ 3.5,\ 2,\ 1.0$ — smaller $\lambda$ (weaker data term) → more smoothing.

### Probabilistic interpretation

$$\arg\min_I\ \lambda D(I, J) + R(I) = \arg\max_I\ \log P(J \mid I) + \log P(I)$$

For the TV model: likelihood $P(J \mid I) = \mathcal{N}\!\left(J;\, I,\, \tfrac{1}{2\lambda}\right)$ and prior $P(I) \propto \exp(-\|\nabla I\|)$.

> **Remark:** the standard deviation of the likelihood model is linked to the weight $\lambda$ in the energy-based formulation.

### Generality of the formulation

The energy-based / probabilistic formulation is very generic; many models fit it:

- $D(I,J)$ or $P(J\mid I)$ encodes **how the noise process works** — it does not have to be additive Gaussian (cf. Rician, speckle from Part I).
- $R(I)$ or $P(I)$ encodes **prior information on what images are plausible**:
  - TV norm — minimal variation
  - Wavelets — sparsity in a transform domain
  - Dictionary learning — composed of sparse atomic elements
  - Unsupervised learning — probabilistic models learned from data
  - Adversarial learning — comparing distribution-wise

## 4.2 Bias correction

### What is bias field?

- An **acquisition artifact**: field inhomogeneity; electrical characteristics of the tissue; poor uniformity of coils; gradients and eddy currents. **Most MRI has it.**
- **Minimal effect on visual interpretation** — actually difficult to see by eye — but **adverse effects on algorithms, especially segmentation**.
- A **non-linear effect on intensities that varies spatially** → remove it as a pre-processing step.
- Appearance: **smoothly varying**; to first-order approximation it does *not* depend on the underlying tissue (in reality it does, but this is mostly ignored).
- Approximate mathematical model — a **multiplicative factor**:

$$j = b\,i + n$$

with $i$ the real image, $b$ the bias field, $n$ noise, $j$ the observed image.

### Basic correction approaches

1. **Hardware side with extra measurements** — ideally a perfect solution, but additional measurements make retrospective data analysis hard and are usually not integrated.
2. **Post-acquisition methods** (very useful):
   - *Manually placed landmarks* — voxels expected to have similar intensity; human in the loop — a very good solution, but needs human interaction.
   - *Joint segmentation* — same structure should have the same intensity in all voxels; works if you know what to expect in the image; geared toward best segmentation accuracy; needs prior information and is too specific.
   - ***N3 algorithm*** — generic, the most popular solution [Sled, Zijdenbos and Evans, *IEEE TMI* 1998] — studied in this lecture.

### N3 algorithm — principle

- Start from $\tilde j(x) = \tilde b(x)\, \tilde i(x) + \tilde n(x)$. Focus on the noise-free case and take **logarithms** to make the bias **additive**: $j(x) = b(x) + i(x)$.
- **Goal**: estimate $b(x)$ at every point; the bias-removed image is then immediately $i(x) = j(x) - b(x)$.
- **Key point**: N3 analyzes the **distributions** of $j$, $b$ and $i$ to understand the effect of the bias term.
- **Assumptions**: (1) independence between pixels when considering the distributions of $j$, $b$, $i$; (2) bias field and image intensities are independent. Write the PDFs as $J(j)$, $B(b)$, $I(i)$.

**Sums of independent random variables** (derivation on slides): for independent $a, b$ with PDFs $f_a, f_b$ and $c = a + b$: compute the CDF $F_c(c') = \Pr(c \le c') = \int_{-\infty}^{\infty} \int_{-\infty}^{c'-b} f_a(a) f_b(b)\, da\, db$, then differentiate using Leibniz' rule twice:

$$f_c(c') = \frac{d}{dc'} F_c(c') = \int_{-\infty}^{\infty} f_a(c' - b)\, f_b(b)\, db$$

→ **the PDF of a sum of independent random variables is a convolution**: $c = a + b \Rightarrow f_c = f_a * f_b$.

**Applied to the bias problem**: $J(j) = \int I(j - b) B(b)\, db = I * B$.

What can we say about $B$? Since $\tilde b \approx 1$, its log $b \approx 0$; so $B$ can be approximated by a **Gaussian with small standard deviation**. Hence $J$ is a **smoothed (blurred) version of $I$**. Also, the bias field is spatially **smooth**.

The estimation must determine $I(i)$ and $b$ given only $j$; N3 divides this into two parts (always assuming $B$ is a small-std Gaussian):

1. **Field estimation** — estimate $b$ given the *distribution* $I(i)$
2. **$I(i)$ estimation** — estimate the distribution itself

### Component 1: field estimation

Strategy: given an intensity value $j$ and the distributions $I(i)$ and $B(b)$, predict the $i$ that corresponds to $j$ *on average*; the bias is the difference. Applied at every pixel/voxel:

$$E[i \mid j] = \int_{-\infty}^{\infty} i\, p(i \mid j)\, di, \qquad b_e(j) = E[b \mid j] = j - E[i \mid j]$$

Derivation (using $b = j - i$ and independence):

$$E[i \mid j] = \frac{1}{J(j)} \int i\, p(i, j)\, di = \frac{\int_{-\infty}^{\infty} i\, B(j - i)\, I(i)\, di}{\int_{-\infty}^{\infty} B(j - i)\, I(i)\, di}$$

All terms are known and $i$ is one-dimensional → solvable numerically with ease.

### Component 2: estimating $I(i)$ — sharpening by inverse filtering

- Key observation: $J(j)$ is a smoothed version of $I(i)$, and $B$ is roughly a small-std Gaussian → approximate $I(i)$ by **sharpening** $J(j)$.
- In Fourier domain: $J = I * B \iff \mathcal{F}\{J\} = \mathcal{F}\{I\}\,\mathcal{F}\{B\}$. Use the (Wiener-style) **deblurring filter**:

$$\hat F = \frac{\mathcal{F}\{B\}^*}{|\mathcal{F}\{B\}|^2 + Z^2}, \qquad \mathcal{F}\{I\} \approx \hat F\, \mathcal{F}\{J\}$$

with $Z$ a small number and $^*$ the complex conjugate. **Note: this is inverse filtering!** So $I(i) \approx \mathcal{F}^{-1}(\hat F \mathcal{F}\{J\})$.

### Two remaining issues

1. **Need for smoothing.** Several noise sources: (1) we ignored the noise $\tilde n$; (2) $\hat F$ is approximate, so the resulting $I$ is too; (3) we haven't yet used the fact that $b(x)$ is a **smooth field** (spatial correlation between neighboring pixels). **Solution**: smooth approximation of the field from **anchor points** using splines or radial basis functions:

$$b(x) = \sum_{n=1}^{N} b_e(j(x_n))\, K(\|x - x_n\|)$$

with $K$ a smoothing kernel, e.g. a Gaussian $\exp(-\|x - x_n\|_2^2/\sigma^2)$.

2. **Need for iterations.** In theory we do not know $B$, we only assume it is Gaussian. If we use too *wide* a $B$, inverse filtering may fail (the estimates must remain valid distributions, $> 0$; deconvolution is hard — illustrated with std 0.1 vs. 0.5). **Solution**: iterate many times with **small Gaussians** (small stds) — not a bad approximation since large Gaussians are convolutions of smaller ones.

### The N3 algorithm (as given)

```
Algorithm: Bias correction with N3
1:  b = 0,  j = log(j̃)                     # observed image, log domain
2:  F̂ = B̂* / (|B̂|² + Z²)                  # deblurring filter
3:  while ||Δb|| ≥ eps do
4:      Estimate J(j) with kernel density estimation or a simple histogram
5:      Compute I(i) = F⁻¹(F̂ · F{J})       # sharpen the histogram
6:      Compute Δb_e = j − E[i|j] at anchor points x_n
7:      Smooth: Δb(x) = Σ_n Δb_e(j(x_n)) K(||x − x_n||)  for all voxels
8:      Update b = b + Δb
9:      Update j = j − Δb
10: end while
```

### Analysis

- Bias correction with N3 is **routinely used for MRI today**; variations exist, e.g. **N4ITK**.
- The method contains multiple approximations; **multiresolution** solutions exist and perform better; you may need to run the tool a few times to get better results.

## 4.3 Variations in image and pixel size

### What is the problem?

- Images may come with different **pixel size** and/or **image size**, and different **field of view (FOV)** (e.g., fine pixel size + small FOV vs. coarse pixel size + large FOV). You may need to compare such "heterogeneous" images.
- Why it happens: differences in acquisitions — in FOV and in pixel size.

### In which applications does it matter?

- **Comparing measurements**: measurements taken in an image correspond to real life; real-life measurements rely on the pixel size; comparisons must take this into account (longitudinal analysis, population statistics, normative distributions).
- **Registration**: aligning images requires building point correspondences; correspondence makes sense only if two points represent similar areas.
- **Machine learning**: model parameters are learned from a set of images; applying the same model to an image with a different image/pixel size or FOV requires attention.
- Effectively important for **all** applications; building algorithms that work for heterogeneous images is one of the most challenging tasks.

### How to tackle it — match the pixel size

Worked example (images of the same person one year apart; displayed sizes proportional to number of pixels):

| | Image 1 | Image 2 | Image 2 resampled |
|---|---|---|---|
| Image size | [454, 384] | [494, 424] | [395, 339] |
| Pixel size | [1.25, 1.25] | [1.0, 1.0] | [1.25, 1.25] |

- Guiding criterion: **measurements taken in the two images should be comparable** → the important factor to match is the **pixel size**.
- **Resample** one image to match the pixel size of the other; resize with a factor determined by the pixel sizes (example Python tool: `scipy.ndimage.zoom`). For volumetric 3D images, match the **voxel size**. **Crop or pad** if necessary to make the image sizes equal.
- **Exercise** (answers given): matching the second image ([494, 424] @ 1.0 mm) to the first ([454, 384] @ 1.25 mm) → resize ratios (x, y) = **0.8, 0.8**. Matching the first to the second → **1.25, 1.25**.
- **Where to find pixel size**: stored in the **DICOM header** and in the headers of many volumetric formats; accessible e.g. via the **`nibabel`** Python package.

### How *not* to tackle it

- A naive approach is to match the **image sizes** directly. Doing so in the example produces image size [454, 384] with effective pixel size **[1.09, 1.11]** — the images are **no longer comparable**, and even the **aspect ratio changed**.

> **Important:** Always match the **pixel/voxel size**. Do **not** match the image size — it may lead to wrong normalization.

---
# 5. Lecture 3 — Spatial Transformations I: Sampling & Interpolation (`lecture3_transformations.pdf`, 57 slides)

*Ender Konukoglu, ETH Zürich, March 3, 2026*

## 5.1 Introduction

- **Registration is an essential task in medical image analysis.** Motivating example: three axial slices of the same individual taken 6 months apart, each showing the same cross-section index (90th slice) of three volumetric images — yet they do **not** show the same anatomy, because the images are not aligned. Image registration aligns them through **spatial transformations** and enables comparisons. Useful for many different applications.
- **Any registration algorithm is composed of four components**; this lecture studies the **"sampling and interpolation model"**.
- Outline: sampling model (how to apply a transformation) · interpolation models · linear transformation models (next lecture) · non-linear transformation models (next lecture).

## 5.2 Sampling model — how to apply a transformation

### Setup

- **Moving image / input image**: the image that will be transformed. **Target image**: the image to which we want to align. Both images have their own pixel coordinates and Cartesian grids.
- Assume a given spatial (coordinate) transformation $x' = T(x)$, with $x$ in the **moving** image's coordinate system and $x'$ in the **target** image's system ($x, x' \in \mathbb{R}^2$ in the examples; 3D is analogous). Also define the inverse $x = T^{-1}(x')$.
- Possible model families: $T(x) = Ax$ (linear) or $T(x) = x + u(x)$ (non-linear).
- In reality, images are **discretized** — we only have values at pixel locations ("a grid of buckets with one intensity in each bucket").

**Main sampling-model question**: given the images, their pixel grids, and the transformation, how do we generate the transformed moving image — i.e., how do we **resample the moving image on the target image's grid**? Two possibilities: **forward** or **backward** transformation.

### Forward transformation

Running example transformation: $T(x) = \{x_1 / [0.15 \sin(2.5 x_2) + 0.85],\ x_2\} = \{x_1', x_2'\} = x'$.

- Continuous, analytic case: transform the input coordinates; everything works nicely.
- **Discrete case**: each pixel of the moving image is transformed via $x' = T(x)$ and its intensity is copied to the output image. **This approach is problematic**:
  - Transformed pixel coordinates $x'$ do not necessarily fall exactly on a pixel.
  - Two pixels can be mapped to the same output pixel.
  - *(and conversely a single input pixel's footprint can spread over multiple output pixels)*
  - **Some output pixels may not get any value assigned** (holes).
- Solution exists but is **quite complicated**: compute output intensities using **partial areas** — the overlap between each transformed pixel and the output pixels is computed and used in a complex intensity-assignment step.

### Backward transformation

Inverse mapping of the example: $T^{-1}(x') = \{x_1' [0.15 \sin(2.5 x_2') + 0.85],\ x_2'\} = \{x_1, x_2\} = x$.

- **For each output (target-grid) pixel**, determine the corresponding location in the input/moving image. This is **much easier to deal with**.
- Remaining problem: the transformed locations of output pixels do not generally fall exactly on input pixels. **Solution: interpolation** — the pixel intensity (all channels if necessary) is interpolated from the nearest input-image pixels.

> **Keep in mind:** Backward transformation is **always easier to use** for transforming images through interpolation.

## 5.3 Interpolation models

**When is interpolation used?** Whenever an image is **resampled**, interpolation computes intensities at the new pixel locations. Example I: applying spatial transformations (backward mapping). Example II: resizing an image during pre-processing (the pixel-size matching example from Lecture 2).

**Problem setup**: intensities are defined at pixel *centers* (black nodes); how do we compute the intensity at an arbitrary point (red node) between them?

### Nearest-neighbor (NN) interpolation

- Assign the intensity value of the **closest pixel** with known intensity.
- Properties (illustrated by rotating an image by 30°):
  - Very **fast**.
  - Does **not introduce new intensity values**.
  - May introduce **heavy artifacts** in gray-level images (visible at object boundaries).
  - Yields a **piecewise-constant** function — neither continuous nor differentiable.
- **On label maps**: NN is *always* used for transforming label images precisely because it introduces no new values; applicable to continuous, discrete, and categorical images.

### Bilinear interpolation

- Uses the **closest four pixels'** intensity values; the 2D extension of linear interpolation; **trilinear** in 3D.
- Construction with corner points $p_{00} = \{x_1^0, x_2^0\}$, $p_{01} = \{x_1^0, x_2^1\}$, $p_{10} = \{x_1^1, x_2^0\}$, $p_{11} = \{x_1^1, x_2^1\}$ and query point $q = \{x_1, x_2\}$ — first linear interpolation along $x_1$:

$$I(x_1, x_2^1) = \frac{x_1^1 - x_1}{x_1^1 - x_1^0} I(x_1^0, x_2^1) + \frac{x_1 - x_1^0}{x_1^1 - x_1^0} I(x_1^1, x_2^1)$$

$$I(x_1, x_2^0) = \frac{x_1^1 - x_1}{x_1^1 - x_1^0} I(x_1^0, x_2^0) + \frac{x_1 - x_1^0}{x_1^1 - x_1^0} I(x_1^1, x_2^0)$$

then along $x_2$:

$$I(x_1, x_2) = \frac{x_2^1 - x_2}{x_2^1 - x_2^0} I(x_1, x_2^0) + \frac{x_2 - x_2^0}{x_2^1 - x_2^0} I(x_1, x_2^1)$$

- Properties:
  - **Fast**.
  - **Introduces new intensity values** (by construction of the weighted average).
  - Artifacts not as big as nearest neighbor.
  - Yields **piecewise-linear** functions — continuous but **not differentiable**.
  - May **smooth/blur** the image → decrease in effective spatial resolution.
  - Trilinear interpolation repeats the procedure along the third dimension using **8 points** (cube corners).
- **On label maps**: produces **extra (spurious) label values** at boundaries — these labels do not exist and must be avoided; never use bilinear for label maps.
- **Alternative formulation**: bilinear interpolation is the parametric surface $I(x_1, x_2) = a + b x_1 + c x_2 + d x_1 x_2$ with 4 unknowns; the 4 corner intensities give a $4\times4$ linear system

$$\begin{bmatrix} 1 & x_1^0 & x_2^0 & x_1^0 x_2^0 \\ 1 & x_1^0 & x_2^1 & x_1^0 x_2^1 \\ 1 & x_1^1 & x_2^0 & x_1^1 x_2^0 \\ 1 & x_1^1 & x_2^1 & x_1^1 x_2^1 \end{bmatrix} \begin{bmatrix} a \\ b \\ c \\ d \end{bmatrix} = \begin{bmatrix} I(x_1^0, x_2^0) \\ I(x_1^0, x_2^1) \\ I(x_1^1, x_2^0) \\ I(x_1^1, x_2^1) \end{bmatrix}$$

solved once, then $a,b,c,d$ interpolate at any point between the corners. Trilinear analogously with **8 unknowns**: $I(x_1,x_2,x_3) = a + bx_1 + cx_2 + dx_3 + ex_1x_2 + fx_1x_3 + gx_2x_3 + hx_1x_2x_3$.

### Bicubic interpolation

- Uses the closest four pixels' information, but requires **derivatives** as well as intensities at those points. Since derivatives are always approximated (from further pixels), the interpolation **effectively uses a larger neighborhood** — commonly the **16 closest points**. Extension of cubic interpolation to 2D; **tricubic** in 3D. Same idea as bilinear: cubic interpolation in one dimension, then the other.
- **Functional form** — 16 unknowns:

$$I(x_1, x_2) = \sum_{i=0}^{3} \sum_{j=0}^{3} a_{ij}\, x_1^i x_2^j$$

A linear system is constructed from information at the 4 closest neighbors: intensities at $p_{00}, p_{01}, p_{10}, p_{11}$ (4 values) + $\partial_{x_1}$ and $\partial_{x_2}$ derivatives at the 4 corners (8 values) + mixed derivatives $\partial_{x_1 x_2}$ at the 4 corners (4 values) = 16 equations. Remarks: computing the derivatives requires further pixels, so bicubic uses more than the closest 4; derivative approximation can be done in different ways.

- **Convolutional form** [R. Keys (1981), "Cubic convolution interpolation for digital image processing"] — interpolation by convolution with the kernel

$$K(x) = \begin{cases} \frac{3}{2}|x|^3 - \frac{5}{2}|x|^2 + 1 & 0 \le |x| \le 1 \\ -\frac{1}{2}|x|^3 + \frac{5}{2}|x|^2 - 4|x| + 2 & 1 \le |x| \le 2 \\ 0 & |x| \ge 2 \end{cases} \qquad f(x) = \sum_{i=-2}^{2} f(x^i) K(x - x^i)$$

applied dimension-by-dimension. In 1D its support is $[-2, 2]$, covering 4 sample points; in 2D Keys' convolution uses the **4×4 closest pixels**. This kernel is called the **Catmull–Rom kernel**.

- Properties:
  - Best at **retaining gray values** — highest interpolation quality among the three.
  - Highest computational requirement.
  - Yields **smooth functions, both continuous and differentiable**.
- **On label maps**: like bilinear, produces spurious extra labels — avoid.

### Comparisons and practice

(Difference images shown: Bilinear−NN, Bicubic−NN, Bicubic−Bilinear.)

- Nearest neighbor yields bad quality (for intensity images); **bilinear is a good trade-off**.
- During registration, interpolation is computed **many times** inside the optimization loop → bilinear's computational advantage matters. A common strategy: **bilinear during optimization, bicubic for the final resampling**.
- For **label maps**, use **nearest neighbor**.
- **A word on kernels**: the generic form $f(x) = \sum_{i=-N}^{N} f(x^i) K(x - x^i)$ admits many different kernels for the same task.

Next week: linear and non-linear transformation models.

---
# 6. Lecture 4 — Transformation Models (`lecture4_transformations.pdf`, 134 slides)

*Ender Konukoglu, ETH Zürich, March 3–10, 2026*

## 6.1 Introduction

- Same motivation as Lecture 3 (three time points of the same individual, unaligned). This lecture studies the **"Transformation Models"** component of the four components of any registration algorithm.
- **Overview of a registration algorithm** (block diagram): target image $I$, moving image $J$ → *spatial mapping and interpolation* produces transformed image $\hat J$ → a *loss/metric* compares $I$ and $\hat J$ → the *transformation model with parameters $\theta$* is updated:

$$\theta^* = \arg\min_\theta \mathcal{L}(I,\ T_\theta \circ J), \qquad T_\theta \circ J \triangleq J(T_\theta(x))$$

- **Main question of the lecture:** how do we parameterize $T_\theta$?
- Recap of the sinusoidal example from Lecture 3 (forward vs. backward mapping). A second example, $T(x) = \{x_1/[0.4\sin(4x_2) - 0.2],\ x_2\}$, shows that a forward transformation may **not even be defined everywhere** (denominator can be 0), while the backward form $T^{-1}(x') = \{x_1'[0.4\sin(4x_2') - 0.2],\ x_2'\}$ is fine — another argument for working with backward/inverse mappings.

## 6.2 Linear transformation models

### General form

$$x' = T(x) = Ax + t$$

defined by matrix $A$ and translation vector $t$.

- **Remark I**: this is *not* strictly linear because of $t$: $T(\alpha x_1 + \beta x_2) \ne \alpha T(x_1) + \beta T(x_2)$. It **is** linear in homogeneous coordinates.
- **Remark II**: these are *global* models, called linear transformation models because the **Jacobian of the transformation is the same for all $x$**.
- Dimensions: 2D→2D: $A \in \mathbb{R}^{2\times2}, t \in \mathbb{R}^2$; 3D→3D: $A \in \mathbb{R}^{3\times3}, t \in \mathbb{R}^3$ (most common cases). 3D↔2D transformations (e.g., 2D ultrasound to 3D CT for intervention) are also studied, but the focus is on the common cases.
- **Parameter counts (generic case)**: 2D: 4 + 2 = **6** free parameters; 3D: 9 + 3 = **12** free parameters. Few parameters compared to non-linear models; the same parameters apply to the entire image, so the count is **independent of the number of pixels/voxels**.
- **Inverse**: if $A$ is invertible, $T^{-1}(x') = A^{-1} x' + t'$ with $t' = -A^{-1} t$.

### Homogeneous coordinates

$$\tilde T(x) = \tilde A \tilde x, \qquad \tilde A = \begin{bmatrix} A & t \\ \mathbf{0} & 1 \end{bmatrix}, \qquad \tilde x = \begin{bmatrix} x \\ 1 \end{bmatrix}$$

In 2D: $\tilde A = \begin{bmatrix} a_{11} & a_{12} & t_1 \\ a_{21} & a_{22} & t_2 \\ 0 & 0 & 1 \end{bmatrix}$. (Question posed: write the 3D version — a $4\times4$ matrix, analogous.)

### Transformations of interest

Not every matrix $A$ is an interesting geometric transformation. Three classes are studied (perspective transformations etc. matter in computer vision/graphics but are not the focus): **rigid body**, **similarity**, **affine**.

#### Rigid body transformation

- One of the most commonly used models in medical image analysis; rotation + translation.
- **2D**: parameterized by one angle $\theta$ and translation $t$ — **3 parameters**:

$$x' = Rx + t, \qquad R = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$$

(rotates the $xy$-plane counterclockwise about the origin by $\theta$).

- **3D**: three rotations + translation; parameterized by angles $\theta_{xy}, \theta_{xz}, \theta_{yz}$ and $t$ — **6 parameters**; $R = R(\theta_{xy}) R(\theta_{xz}) R(\theta_{yz})$ where each angle rotates about another axis: $\theta_{xy}$ = rotation about the $z$-axis (rotation of the $xy$ plane), $\theta_{xz}$ = about the $y$-axis, $\theta_{yz}$ = about the $x$-axis:

$$R(\theta_{xy}) = \begin{bmatrix} \cos\theta_{xy} & -\sin\theta_{xy} & 0 \\ \sin\theta_{xy} & \cos\theta_{xy} & 0 \\ 0 & 0 & 1 \end{bmatrix} \quad R(\theta_{xz}) = \begin{bmatrix} \cos\theta_{xz} & 0 & \sin\theta_{xz} \\ 0 & 1 & 0 \\ -\sin\theta_{xz} & 0 & \cos\theta_{xz} \end{bmatrix} \quad R(\theta_{yz}) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos\theta_{yz} & -\sin\theta_{yz} \\ 0 & \sin\theta_{yz} & \cos\theta_{yz} \end{bmatrix}$$

*(the slide's $R(\theta_{yz})$ misprints its first row; the correct x-axis rotation is shown here)*

- **Question**: Is $R(\theta_{xy})R(\theta_{yz})R(\theta_{xz}) = R(\theta_{xy})R(\theta_{xz})R(\theta_{yz})$? **No** — rotation matrices do not commute.
- Examples (2D rigid, 3D rigid): the transformation can push the object **out of the FOV**; a 3D rigid transformation can **change the visible anatomy in a given slice** — essentially a different cross-section occupies the same slice after transformation. (Interpolation: bilinear/trilinear.)
- **Center of rotation is important**: the centers of the coordinate systems in both domains must be defined. In the examples the center was the image center; choosing e.g. the image origin changes the resulting transformation. This applies to all transformations and is especially important for linear ones, where the same matrix applies to the whole image.

#### Similarity transformation

- Builds on rigid body motion by integrating **isotropic scaling**; also used extensively.

$$x' = R S x + t, \qquad S = \begin{bmatrix} s & 0 \\ 0 & s \end{bmatrix} \text{ or } \begin{bmatrix} s & 0 & 0 \\ 0 & s & 0 \\ 0 & 0 & s \end{bmatrix}$$

- Scaling "zooms" in or out. Adds **one parameter** to rigid: **4 parameters in 2D, 7 in 3D**.
- **Question**: is $x' = SRx$ the same as $x' = RSx$? **Yes** — with isotropic scaling, the order of rotation and scaling does not matter.

#### Similarity with anisotropic scaling

- Different scale factors per dimension: $S = \mathrm{diag}(s_1, s_2)$ or $\mathrm{diag}(s_1, s_2, s_3)$; $x' = RSx + t$.
- **No longer a similarity transformation — shapes are not preserved.**
- Parameters: 2D: 1 (rotation) + 2 (translation) + 2 (scaling) = **5**; 3D: 3 + 3 + 3 = **9**.
- **Question**: is $SRx$ the same as $RSx$? **No** — with anisotropic scaling the order matters a lot; applying $T(x) = SRx + t$ with the same parameters yields **shearing** ("wrong order gives shearing").

#### Affine transformation

- Extends anisotropic scaling with **shearing**:

$$x' = W R S x + t, \qquad W = \begin{bmatrix} 1 & w \\ 0 & 1 \end{bmatrix} \text{ or } W = \begin{bmatrix} 1 & w_1 & w_2 \\ 0 & 1 & w_3 \\ 0 & 0 & 1 \end{bmatrix}$$

- Shapes are no longer preserved. Parameters: 2D: 1 + 2 + 2 + 1 (shear) = **6**; 3D: 3 + 3 + 3 + 3 = **12**. Alternative parameterizations possible.
- A **full** affine transformation would also include **reflection**, achievable with a negative scaling factor.
- Affine with shearing is **rarely used** in medical image analysis; the shearing model is not used often.

### Application areas of linear transformations (not exhaustive)

| Model | Applications |
|---|---|
| **Rigid** (rotation + translation) | longitudinal analysis (aligning temporal sequences); motion correction (rigid motion between slices in a volumetric acquisition); multi-modal registration (aligning images of different modalities acquired at the same time, e.g., PET/MRI/CT) |
| **Similarity** (+ global scaling) | spatial normalization for machine-learning applications (removing variations that are easy to eliminate); alignment for shape analysis (eliminating global shape variation) |
| **Similarity + anisotropic scaling** | initialization for non-linear registration; spatial normalization for ML |
| **Affine** (+ shear) | initialization for non-linear registration; studying mechanical tissue properties; mechanical modeling of intervention; used to define more complicated non-linear transformations (e.g., poly-affine) |

**Summary**: linear transformations are used extensively; rigid, similarity, and similarity+anisotropic scaling are common in medical image computing; affine is common in modeling interventions; all are easy to implement.

## 6.3 Non-linear deformable transformation models

### General form

$$x = T^{-1}(x') = x' + u(x')$$

defined by a (possibly non-linear) displacement function $u(x')$. Notes:

- We **directly model the inverse transformation** (target → moving domain), because establishing the inverse of an arbitrary forward transformation is difficult, and the sampling model works best with inverse/backward mappings.
- Core question: **how to parameterize $u(x')$?** The lecture follows the survey **Sotiras, Davatzikos, Paragios, IEEE TMI 2013**.

### Naive parameterization

- One displacement vector per pixel/voxel, independent of each other. Parameters: 2D: (#pixels)×2; 3D: (#voxels)×3.
- Issues: (1) **very large number of parameters/dof** — difficult to estimate; (2) can generate very **"noisy" transformations** that may be unrealistic and can even **change the topology** of the underlying image. Example shown: $u(x) \sim \mathcal{N}(0, 1.5 I_2)$ i.i.d. per pixel → noisy displacement field and a noisy moved image — undesirable.
- During registration optimization we want to search only through **smooth transformations** — achieved via the parameterization, with models that generate smooth displacement fields **for any parameter values**, effectively reducing the number of free parameters. Two strategies: **interpolation-based** and **physical-model-based**.

### Interpolation-based methods

**Main idea:** define displacement vectors on a **sparse set of control points** and interpolate in between with a smooth model. Radial basis function form:

$$u(x') = \sum_{n=1}^{N} \phi(|x' - x_n'|)\, d_n, \qquad \phi: \mathbb{R}^+ \to \mathbb{R},\ d_n \in \mathbb{R}^{2\text{ or }3}$$

with control points $x_n'$, basis functions $\phi$, and coefficients $d_n$ defining the transformation. Two commonly used forms: **Thin-Plate Splines (TPS)** and **Free-Form Deformations (FFD) with B-splines**.

#### Thin-plate splines (TPS)

- Basis function: $\phi(r) = r^2 \log r$.
- Sometimes combined with an affine transformation: $x = Ax' + t + \sum_{n=1}^N \phi(|x' - x_n'|) d_n$ — the affine part is the global transformation, TPS the local refinement.
- Control points **do not have to be uniformly spaced** — they can be randomly dispersed.
- **Parameterization**: in landmark-based registration (where TPS is used extensively) control points are given by the user; the parameters are the coefficients $\{d_n\}$: 2D: (#control points)×2, 3D: ×3 (plus affine parameters if included). → a non-linear deformable model with **very few parameters**.
- Examples: TPS with 50 random control points produces smooth transformations that deform the object non-linearly. *(Quiz: with 50 control points in 2D, the TPS model has 100 parameters.)* Increasing control points (25 → 100 → 400) gives more complicated transformations.
- **Analysis**:
  - Transformations are bound to be smooth due to the interpolating kernel $\phi$.
  - The effect of each control point is **global** ($r^2\log r$ grows with distance).
  - More control points → more complicated transformations.
  - Mostly used for **landmark-based** registration, not intensity-based: given point correspondences, model parameters are found to satisfy them.

#### Free-form deformation (FFD) with B-splines

- In 3D:

$$u(x') = \sum_{l=0}^{3} \sum_{m=0}^{3} \sum_{n=0}^{3} B_l(\mu_1) B_m(\mu_2) B_n(\mu_3)\, d_{i+l,\ j+m,\ k+n}$$

- $N_x \times N_y \times N_z$ control points on a **uniform grid** with spacing $\delta$ (image grid in gray, FFD control grid in red on the slide). Displacements $d$ live **only on control points**; $u(x')$ is interpolated at image grid points.
- Index and local-coordinate definitions:

$$i = \left\lfloor \frac{x_1'}{\delta} \right\rfloor - 1, \quad j = \left\lfloor \frac{x_2'}{\delta} \right\rfloor - 1, \quad k = \left\lfloor \frac{x_3'}{\delta} \right\rfloor - 1; \qquad \mu_c = \frac{x_c'}{\delta} - \left\lfloor \frac{x_c'}{\delta} \right\rfloor \ (c = 1,2,3)$$

- **Cubic B-spline basis functions** (order 4):

$$B_0(s) = \frac{(1-s)^3}{6}, \quad B_1(s) = \frac{3s^3 - 6s^2 + 4}{6}, \quad B_2(s) = \frac{-3s^3 + 3s^2 + 3s + 1}{6}, \quad B_3(s) = \frac{s^3}{6}$$

  Continuous at the anchor points **up to the 2nd derivative** — consequently the transformation has continuous Jacobian and divergence fields.
- Examples: FFD with 25 control points gives smooth transformations even when control-point displacements are large. Increasing control points (25 → 100 → 400) gives **less smooth** transformations.
- **Parameterization**: control grid is often subsampled from the image grid; parameters are the coefficients $\{d_n\}$ — (#control points)×2 in 2D, ×3 in 3D (same as TPS). Can include an affine component like TPS; one can also optimize control-point locations (more parameters).
- **Analysis**:
  - Smooth by construction (interpolating kernel).
  - Effect of each control point is **local** (finite support) — unlike TPS.
  - Finer control grids → less smooth transformations → enables **multi-resolution** strategies: start with few control points, then increase.
  - Mostly used for **intensity-based** registration (not necessarily landmark-based).

### Physical models (pixel/voxel-wise)

- A displacement vector for **each grid point**, but constrained to satisfy a **physical model** (fluids, elastic bodies, …): very large number of parameters, yet restricted transformations → lower *effective* degrees of freedom.
- Coarse classification: (1) elastic body models, (2) fluid flow models, (3) curvature registration, (4) diffusion models, (5) flows of diffeomorphisms.
- **PDE-based examples** ($u(x')$ must satisfy a model equation; $F(x')$ = user-defined force field):
  - *Linear elastic body model*: $\mu \Delta u(x') + (\mu + \lambda) \nabla(\nabla \cdot u(x')) + F(x') = 0$ — $\Delta$ Laplace operator, $\mu$ rigidity, $\lambda$ Lamé's first coefficient, $\nabla$ gradient, $\nabla\cdot$ divergence.
  - *Diffusion model*: $\Delta u(x') + F(x') = 0$ — displacement field satisfies the **Poisson equation**.
  - *Curvature registration*: $\Delta^2 u(x') + F(x') = 0$ (biharmonic).
- **Flow models**: the motion of each point is an ODE

$$\frac{dx}{dt} = v(x, t), \qquad x(0) = x'$$

driven by a time-dependent **velocity field** $v(x,t)$; solving by integration in time gives $x = \int_0^1 v(x,t)\,dt + x' = u(x') + x'$. ODE theory: if $v(x,t)$ is smooth, the resulting transformation is a **diffeomorphism**. This framework leads to the **LDDMM** (Large Deformation Diffeomorphic Metric Mapping) registration algorithm. Parameter count is even higher — one velocity per pixel/voxel *and* time — so regularization models on $v(x,t)$ are used.

- **Flows with stationary velocity fields (SVF)**: $v$ does not depend on time; $x = \int_0^1 v(x)\,dt + x' = u(x') + x'$ — fewer parameters (one velocity vector per pixel/voxel), and **efficient integration schemes exist** for stationary fields (e.g., scaling-and-squaring).

Next week: loss functions and registration algorithms — landmark-based and intensity-based registration.

---
# 7. Lecture 5 — Loss Metrics and Registration Algorithms (`lecture5_registration.pdf`, 121 slides)

*Ender Konukoglu, ETH Zürich, March 17, 2026*

Completes the four registration components: after *sampling & interpolation* (Lecture 3) and *transformation models* (Lecture 4), this lecture covers **metric/loss functions** and (briefly) **optimization**. Recurring block diagram: $\theta^* = \arg\min_\theta \mathcal{L}(\theta)$ with $\mathcal{L}$ comparing target $I$ and transformed moving image $\hat J = T_\theta \circ J$.

## 7.1 Classifying registration problems

Classification in **two axes**:

| | Same subject's images | Different subjects' images |
|---|---|---|
| **Same modality** | intra-subject, intra-modality *(easiest — green)* | inter-subject, intra-modality |
| **Different modality** | intra-subject, inter-modality | inter-subject, inter-modality *(hardest — red)* |

- Same vs. different **subjects** → the alignment strategy changes; the *transformations* differ.
- Same vs. different **modalities** → need different *loss functions* (intensity-based losses that are unaffected by modality variations).
- Difficulty color-coding on the slide: green easiest, red hardest; the ordering of the two middle cells is a subjective judgement. Difficulty also depends on **anatomy** — e.g., brain is easier than abdomen.
- **Intra-subject registration**: aligning two images of the same person; many applications; easier because the same anatomy is expected. *But it can still be difficult*: 3D+time lung MRI — deformation during motion can be very complex and transformation models may not capture it (e.g., breathing motion and its effect on the abdomen); images of the same individual may differ vastly in intensity characteristics (e.g., neuroimaging: structural MRI vs. fractional anisotropy vs. T2\*).
- **Inter-subject registration is often difficult**: differences in anatomies even when images show the same structures; **lack of exact correspondence** between two images; **presence of pathologies** (e.g., brain tumor) makes registration even harder.

## 7.2 Landmark-based registration

### Landmarks

- Dictionary definition (OED): a conspicuous object that serves as a guide — originally for sailors in navigation.
- **Landmarks in medical images**: prominent locations; distinct from surroundings (similar to *interest points* in computer vision); easily identifiable; used to orient ourselves in an image; depend on the imaged structure and image type; most commonly **manually defined with domain knowledge**. Examples for the thorax: **trachea bifurcation**, most superior tip of the liver, tips of the pelvis bones (iliac crests).
- Landmarks must be **identifiable across images** to define correspondence: corresponding landmarks guide the registration; the correspondence between landmarks defines correspondence for **all other pixels/voxels**; the image modalities constrain which landmarks are usable.
- Scenarios discussed:
  - *Same modality, different subjects* (two CTs, similar protocols): natural landmarks are anatomical structures common to every person AND visible in CT.
  - *Different modality, different subjects* (CT + MRI): landmarks must be identifiable in **both** modalities.
  - *Same person, same modality* (two time points): besides common anatomical landmarks, **sample-specific structural variations** can serve as landmarks (blue points on slide) → more accurate alignment. Same person + different modalities is also very common; same idea applies.
  - *Biological samples*: same concepts apply, e.g., OCT vs. Nissl-stained microscopy of the same tissue sample; constellations of neurons used as corresponding structures; defining such landmarks **across different tissue samples** can be difficult.
  - Landmarks must be defined in **3D** for volumetric medical images (2D used here for illustration).

### Landmark-based loss function

$$\mathcal{L}(\theta) = \sum_{n=1}^{N} \left\| x_n^{(1)} - T_\theta\left(x_n^{(2)}\right) \right\|_2^2, \qquad x \in \mathbb{R}^d$$

- $x_n^{(1)}$: $n$-th landmark in the first image; $x_n^{(2)}$: corresponding landmark in the second; $T_\theta(x_n^{(2)})$: transformed landmark; $\theta$: transformation parameters; $d$: problem dimension (2D, 3D, 3D+time, …); $\|\cdot\|_2^2$ the usual $\ell_2$ loss — other losses possible.

### Linear alignment

With $T_\theta(x) = Ax + t$:

$$\arg\min_{A, t} \sum_{n=1}^{N} \left\| x_n^{(1)} - \left(A x_n^{(2)} + t\right) \right\|_2^2$$

Once $A^*, t^*$ are determined they apply to **any** pixel: $x^{(1)} = A^* x^{(2)} + t^*$.

- **Exercise** (posed): this minimization has an analytical (closed-form least-squares) solution — derive it. For loss functions without analytical solutions, numerical optimization is used.
- **Example I** — rigid, same subject, same modality: using only **4 landmarks**, rigid registration correctly aligns the images; the overlay becomes sharper (= good alignment).
- **Example II** — rigid, different subjects, same modality (landmarks: trachea bifurcation, liver tip, pelvis tips): rigid registration **may not be enough** to account for inter-subject differences; landmarks do not match perfectly.
- **Effect of the number of landmarks** (affine in 2D):
  - With **3 landmarks**: landmark alignment is *perfect* but the rest of the image is not — the 2D affine transformation has **6 unknowns**, and 3 correspondences give exactly **6 equations** (each coordinate gives one) → a well-determined system.
  - **More landmarks** → worse alignment *of the landmarks* themselves but **better alignment elsewhere** — an **over-determined** system solved in the least-squares sense.
  - Why can't affine align all landmarks perfectly? With 6 parameters it lacks the degrees of freedom to satisfy more than 3 correspondences in 2D.

### Non-linear alignment with thin-plate splines (TPS)

Reminder of the TPS model (Lecture 4): $u(x') = \sum_n \phi(|x' - x_n'|) d_n$ with $\phi(r) = r^2 \log r$, optionally with a global affine part; control points can be placed anywhere; TPS is extensively used for landmark-based registration.

Using the **landmarks as control points**, the cost becomes:

$$\mathcal{L} = \sum_{n=1}^{N} \left\| x_n^{(1)} - x_n^{(2)} - \sum_{k=1}^{N} \phi\left(|x_n^{(2)} - x_k^{(2)}|\right) d_k \right\|_2^2$$

minimized over the coefficients $\{d_k\}_{k=1}^N$. After finding $d_k^*$, the transformation is defined **everywhere** in the target image, not just at landmarks:

$$T_\theta(x^{(2)}) = x^{(2)} + \sum_{k=1}^{N} \phi\left(|x^{(2)} - x_k^{(2)}|\right) d_k^*$$

- **Exercise** (posed): can this minimization be solved analytically? (Hint: write everything in matrix form — yes, it is linear in $d_k$.) Other losses may require numerical optimization.
- **Example I** — TPS, same subject, same modality, 5 landmarks: sharper overlay, good alignment.
- **Example II** — TPS, different subjects, same modality, **12 landmarks**: TPS has larger degrees of freedom and models complicated transformations; landmarks are perfectly aligned *and* the rest of the image aligns much better than with linear transformations. Even more landmarks → better alignment. TPS can model deformable/non-linear transformations.
- **Comparison**: using the same 12 landmarks, TPS vs. affine leads to different results — TPS aligns better.

### Further notes on landmark-based registration

- Other transformation models can be used in the same optimization.
- Landmarks were placed manually here, but **automatic landmark detection** with machine learning is possible.
- **More robust losses** than $\ell_2$ (e.g., $\ell_1$) can be used — especially important when landmark quality is suspect (e.g., automatically detected landmarks).
- Identifying all interest points then using **robust fitting** (e.g., **RANSAC**) is also possible — particularly common for natural images, less so for medical images.

## 7.3 Intensity-based registration

$$\mathcal{L}(\theta) = d(I,\ T_\theta \circ J)$$

with $d(\cdot,\cdot)$ a distance metric between intensities. Characteristics: **no landmarks** — no input information about corresponding locations; a **harder problem with no unique solution**; **more generally applicable** since no landmark placement is required; the definition of $d$ is very important and depends on the type of registration.

### Intra-modality metric: Sum of Squared Differences (SSD)

$$\mathcal{L}(\theta) = \sum_{x\in\Omega} \|I(x) - (T_\theta \circ J)(x)\|_2^2 = \sum_{x\in\Omega} \left\| I(x) - J(T_\theta^{-1}(x)) \right\|_2^2$$

($\Omega$ = domain of the fixed image.)

- Very intuitive: SSD is 0 when images are identical, higher otherwise.
- **No unique solution**: one can warp images in arbitrary ways to perfectly minimize SSD.
- **Assumes intensities are comparable across images** → does not work for inter-modality registration.

**Image gradients are important for SSD** — differentiate the loss w.r.t. a parameter $\theta$:

$$\frac{d\mathcal{L}}{d\theta} \propto -\sum_{x\in\Omega} \left(I(x) - J(T_\theta^{-1}(x))\right)\, \nabla J\left(T_\theta^{-1}(x)\right)^T \frac{d T_\theta^{-1}(x)}{d\theta}$$

The right-hand dot product $\nabla J^T \frac{dT^{-1}_\theta}{d\theta}$ is non-zero only when the change of the transformation has a **component parallel to the image gradient** → *only transformation changes parallel to image gradients can change the loss value* (movements along iso-intensity directions are invisible to SSD).

**Variations on SSD**: robust losses instead of $\ell_2$ (e.g., $\ell_1$ — optimization may become harder); or compare **features extracted from patches** instead of raw intensities:

$$d(I, T_\theta \circ J) = \sum_{x\in\Omega} \left\| f(I(N(x))) - f\left(J(N(T_\theta^{-1}(x)))\right) \right\|_2^2$$

with $f$ a feature map and $N(\cdot)$ a patch centered at the argument.

### Inter-modality loss metrics

SSD assumes comparable intensities; across modalities (e.g., structural MRI vs. T2\*) this fails. Inter-modality metrics are **statistical** — they assume a *statistical relationship* between the intensities of the two images. The two most common: **normalized cross-correlation** and **mutual information**.

#### Normalized Cross-Correlation (NCC)

$$\mathcal{L}(\theta) = -\mathrm{NCC}^2(I, J), \qquad \mathrm{NCC}(I,J) = \frac{1}{|\Omega|}\frac{1}{\sigma_I \sigma_J} \sum_{x\in\Omega} (I(x) - \mu_I)\left(J(T_\theta^{-1}(x)) - \mu_J\right)$$

with $|\Omega|$ the number of pixels of the fixed image, $\mu_I, \mu_J, \sigma_I, \sigma_J$ the means/standard deviations of $I$ and $T_\theta \circ J$.

- NCC is the image version of the **Pearson correlation coefficient**; assumes pixel intensities are independent random variables; approximates $\mathbb{E}[(I - \mu_I)(J - \mu_J)]/(\sigma_I \sigma_J)$.
- Achieves its maximum of **1** iff $I(x)$ and $J(T_\theta^{-1}(x))$ are **linearly related with the same relationship for all** $x \in \Omega$.
- NCC can be positive or negative — **both are fine** (any linear relationship indicates alignment) → the loss uses $\mathrm{NCC}^2$, minimizing its negative.
- **NCC in action**: T1w vs. T2w images, originally aligned; NCC computed in the foreground as one image is rotated ±20° — clear optimum at 0°, with intensity scatter plots at 0° and 10° rotation shown. Note: NCC does not reach −1 in reality because the relationship between all pixels is never perfectly linear (e.g., background pixels share the same intensity in both images and don't obey the linear model of the foreground).

#### Mutual Information (MI)

$$\mathrm{MI}(I, J) = D_{KL}\big(p(I, J)\,\|\, p(I)\,p(J)\big) = \sum_{I \in \mathcal{I}} \sum_{J \in \mathcal{J}} p(I, J) \log \frac{p(I, J)}{p(I)\, p(J)}$$

- $D_{KL}$: Kullback–Leibler divergence (distance between two distributions); $p(I), p(J)$: intensity histograms (marginals); $\mathcal{I}, \mathcal{J}$: intensity ranges; $p(I,J)$: **joint histogram**. MI = KL divergence between the joint intensity distribution and the product of the marginals.
- Computation assumption: each pixel/voxel is an independent random sample of the intensity random variables — $p(I)$ from the $I(x)$ values, $p(J)$ from $J(T_\theta^{-1}(x))$ values, $p(I,J)$ from the **paired** values $(I(x), J(T_\theta^{-1}(x)))$. When $T_\theta$ changes, the joint distribution changes substantially.
- Interpretation: MI = 0 iff $p(I,J) = p(I)p(J)$ — intensities independent, no statistical relationship; coarsely, knowing $I(x)$ then provides **no information** about $J(T_\theta^{-1}(x))$ and vice versa. Unaligned images → little statistical relationship → low MI; perfectly aligned images → strong relationship → **MI is maximized** during registration.
- Practical computation: approximate marginals with histograms of $I$ and $T_\theta \circ J$, joint with the **2D histogram**; then $\mathrm{MI} = \sum_i \sum_j h(i,j) \log\frac{h(i,j)}{h(i)h(j)}$ over histogram bins.
- **MI in action**: same T1w/T2w rotation experiment — sharp peak at 0°.

#### NCC vs. MI

- **MI is more powerful** — captures complicated (non-linear) relationships; e.g., no ripples at large rotation angles in the example. But MI **can be difficult to compute**.
- **NCC is easier to compute**.

## 7.4 Optimization (briefly)

Cost functions seen: landmark-based $\arg\min_\theta \sum_n \|x_n^{(1)} - T_\theta(x_n^{(2)})\|_2^2$; intensity-based $\arg\min_\theta \mathrm{SSD}$, $\arg\max_\theta \mathrm{NCC}^2$, $\arg\max_\theta \mathrm{MI}$.

- Optimization for registration is difficult in general. Landmark-based is the easier of the two (as long as the transformation's dof is consistent with the number of landmarks). Linear models are easier to optimize than non-linear.
- **Deformable registration** (intensity-based + non-linear transformations) is **ill-posed** — many different solutions give similar results → **regularization**.

### Regularization

$$\mathcal{L}(\theta) = d(I, T_\theta \circ J) + \lambda\, r(T_\theta^{-1}), \qquad T_\theta^{-1} = x + u(x)$$

$r$ prefers some transformations over others; $\lambda$ is a weight.

- **Tikhonov-style** schemes preferring smoothly varying $u(x)$; generic form $r = \int_\Omega \phi(\nabla u)\, dx$ with $\phi$ convex, minimum at 0. Examples:
  - $\ell_2$ regularization — prefers smoothly varying displacement fields: $r = \int_\Omega (\nabla u)^2 dx$
  - $\ell_1$ regularization — prefers piecewise-constant $u(x)$: $r = \int_\Omega |\nabla u|\, dx$
- **Continuum-mechanics energies**:
  - *Linear elastic regularization* — Cauchy strain tensor $V(u) = (\nabla u + \nabla u^T)/2$; minimizes strain energy:
    $$r(T_\theta^{-1}) = \int_\Omega \lambda\, \mathrm{Tr}(V)^2 + \mu\, \mathrm{Tr}(V^2)\, dx$$
    with Lamé constants $\lambda, \mu$; effectively prefers **small** $u(x)$.
  - *Hyperelastic regularization* — allows larger deformations; simplest material model: **Saint Venant–Kirchhoff** with the Green–St. Venant strain tensor
    $$E(u) = \tfrac{1}{2}\left(\nabla T_\theta^T \nabla T_\theta - I\right) = \tfrac{1}{2}\left[\nabla u^T + \nabla u + \nabla u^T \nabla u\right], \qquad r = \int_\Omega \tfrac{\lambda}{2} \mathrm{Tr}(E)^2 + \mu\, \mathrm{Tr}(E^2)\, dx$$
    which prefers **locally rigid** transformations.

### Complete model and optimizers

- Linear registration: $\mathcal{L}(\theta) = d(I, T_\theta \circ J)$ with $T_\theta = Ax + t$.
- Non-linear registration: $\mathcal{L}(\theta) = d(I, T_\theta \circ J) + \lambda r(T_\theta^{-1})$ with $T_\theta^{-1} = x + u(x)$.
- Most commonly optimized with **continuous methods**: $\theta_{t+1} = \theta_t + \alpha\, g(\mathcal{L}(\theta_t))$ with step size $\alpha$ and gradient-based $g$; simplest is **gradient descent** $\theta_{t+1} = \theta_t - \alpha \frac{d\mathcal{L}}{d\theta}\big|_{\theta_t}$. Second-order schemes, discrete formulations, and gradient-free techniques are also possible.

### Practical notes

1. **Sequential optimization**: first align with **linear** registration, then **deformable** non-linear registration — gives better alignment.
2. **Multi-scale optimization**: even with regularization, registration can get stuck in **local minima**. Build an image pyramid; registration starts at the **coarsest scale** (easier, fewer details), and each scale's result $\theta^{(k)}_*$ initializes the next finer scale $\theta^{(k-1)}_0 = \theta^{(k)}_*$.
3. **Demonstration**: fixed/target and moving/source images; affine result; then FFD-with-B-splines after affine.
4. **Toolboxes** (coding a full registration tool is laborious): **ITK** (C++ library with registration tools); **elastix** (http://elastix.isi.uu.nl — great and easy to use); **ANTs** (Advanced Normalization Tools, http://stnava.github.io/ANTs/).

---
# 8. Lecture 6 — Introduction to Segmentation (`lecture6_segmentation_introduction.pdf`, 131 slides)

*Ender Konukoglu, ETH Zürich, March 24, 2026*

**Segmentation = from image intensities to anatomical structures and semantic information.** Recap of the principle: assign a label $L(x) \in \{0,\dots,N\}$ to each pixel/voxel from features $f(x) = [I(x), I(N(x)), J(N(x)), \dots]$, $L(x) \approx S(f(x))$; labels can be organs, parts of organs, or lesions. The techniques-overview taxonomy from Lecture 1 is repeated (thresholding/histogram, clustering, graph partitioning, region growing, variational/PDE-based, discriminative, generative).

**Lecture overview**: (1) Basic segmentation — thresholding, K-means, EM; (2) Atlas-based segmentation — formulation, multi-atlas; (3) Spatial constraints — morphological operations, random field priors.

## 8.1 Basic segmentation

### Thresholding

$$L(x) = \begin{cases} 1 & I(x) > \tau \\ 0 & I(x) \le \tau \end{cases} \quad \text{(or vice-versa)}$$

- Particularly useful for easy **foreground–background subtraction**; a preprocessing method for region-of-interest determination; used **routinely in histology** and other microscopy where stains provide the necessary contrast.
- **Determining the threshold**: histogram analysis (per-channel histograms of RGB histology images shown); automatic methods find peaks/troughs of the histogram. **Otsu's method**: choose the threshold minimizing the intra-class variance

$$\min\ p_1 \sigma_1^2 + p_2 \sigma_2^2$$

where $p_{1,2}$ are the numbers of pixels in fore-/background and $\sigma^2_{1,2}$ the intensity variances within the groups.

- Remarks: **global** thresholding (entire image) vs. **local** (per ROI/patch); image noise can create isolated FG/BG islands; multiple objects require multiple thresholds; a nice generalization is the **K-means algorithm**.

### K-means clustering

- **Unsupervised** clustering: voxels/pixels are clustered according to features — intensity, color, temporal sequence, other features (gradient, wavelet transform, …); automatically assigns each pixel to one of $N$ clusters.
- Goal: distribute pixels into $N$ groups such that features are **homogeneous within groups** (reducing within-group feature variance), taking multiple features into account; no information about structures or groups (unsupervised). Iterative: assign pixels to groups → compute group means → re-assign by distance to group centers.

```
Algorithm: K-Means
1: Describe each pixel by features f(x) ∈ R^m (intensity only: m=1; RGB: m=3)
2: Randomly choose N points as group centers m_i^0 ∈ R^m, i = 1..N
3: while ||m_i^t − m_i^{t−1}|| > ε for any i do
4:     Assign groups: c(x) = arg min_i ||f(x) − m_i||₂
5:     Recompute means: m_i = Σ_x δ_{i=c(x)} f(x) / Σ_x δ_{i=c(x)}
6: end while
```

- Examples: MRI clustered with 2, 3, 4 clusters; a question slide shows the same image and number of clusters giving **different results** on two runs ("Why does this happen?" — random initialization / local minima).
- **Remarks**:
  - Creates clusters with features as homogeneous as possible; finds assignments and means that best explain the data for the given number of clusters.
  - Very easy to implement.
  - The **choice of the number of clusters** has a major influence; automatic selection methods exist (variance-based heuristics; non-parametric Bayesian methods).
  - **Initialization matters** — K-means can get stuck in local minima → run multiple times with different initializations; multi-scale methods for robustness.
  - A **probabilistic formulation** is possible ($\max \ln p(I)$) — more robust and allows extensions → leads to EM.

### Expectation–Maximization (EM) segmentation

**Mixture model** — same idea as K-means but probabilistic; this view later enables accurate atlas-based segmentation and motivates registration. Assume all pixels independent; model intensities/features $f$ as a mixture:

$$p(f) = \sum_{n=1}^{N} p(f \mid c = n)\, p(c = n)$$

- $p(c)$: prior probability of the (latent) label — the **mixture coefficients**; $p(f \mid c)$: likelihood, often Gaussian:

$$p(f \mid c = n) = \mathcal{N}(f \mid \mu_n, \Sigma_n) = \frac{1}{\sqrt{2\pi |\Sigma_n|}} \exp\left(-\tfrac{1}{2}(f - \mu_n)^T \Sigma_n^{-1} (f - \mu_n)\right)$$

with class-specific $\mu_n, \Sigma_n$ → **Gaussian mixture model (GMM)**.

- 1D example with 3 latent labels and different mixture weights ($p(c{=}0){=}p(c{=}1){=}p(c{=}2){=}1/3$; $3/5$–$1/5$–$1/5$; etc.). Comparing with a real brain-MR histogram: a GMM "may not be a bad approximate model for the intensities."

**Fitting**: unsupervised maximum likelihood, assuming a known number of classes $N$ and pixel independence:

$$\max_\theta \log p(I \mid \theta) = \max_\theta \sum_{x\in\Omega} \log \sum_{n=1}^N p(I(x) \mid c(x) = n)\, p(c(x) = n)$$

with $\theta = \{\pi_n = p(c(x){=}n),\ \mu_n,\ \Sigma_n\}$. After fitting, segmentation is via the **posterior**: $c^*(x) = \arg\max p(c(x) \mid I(x))$. Gradient descent is possible but difficult; the better alternative is **EM**.

**Key idea — two alternating steps**: *E-step*: assume likelihood parameters, "softly" assign samples to labels. *M-step*: given soft assignments, maximize likelihood parameters (e.g., best Gaussian means/stds).

**Derivation** (dropping $x$): using Bayes' rule, $\log p(I \mid \theta) = \log p(I, c \mid \theta) - \log p(c \mid I, \theta)$. Taking expectations w.r.t. $p(c \mid I, \theta^{old})$:

$$\log p(I \mid \theta) = \underbrace{\mathbb{E}_{p(c \mid I, \theta^{old})}[\log p(I, c \mid \theta)]}_{Q(\theta \mid \theta^{old})} - \mathbb{E}_{p(c \mid I, \theta^{old})}[\log p(c \mid I, \theta)]$$

One can show $0 \ge \mathbb{E}_{p(c\mid I,\theta^{old})}[\log p(c \mid I, \theta^{old})] \ge \mathbb{E}_{p(c\mid I,\theta^{old})}[\log p(c \mid I, \theta)]\ \forall\theta$, which implies: (1) $\log p(I \mid \theta) \ge Q(\theta \mid \theta^{old})$; (2) **any $\theta$ that increases $Q$ also increases $\log p(I\mid\theta)$** → iterate:

- **E-step**: compute $p(c \mid I, \theta^{old})$
- **M-step**: maximize $Q(\theta \mid \theta^{old}) = \mathbb{E}_{p(c\mid I, \theta^{old})}[\log p(I, c \mid \theta)]$; set $\theta^{old} = \theta^*$
- Start from random $\theta^{old}$, iterate E and M steps.

**For the GMM** the steps are closed-form:

- E-step (responsibilities):

$$p(c(x) \mid I(x), \theta^{old}) = \frac{\mathcal{N}(I(x) \mid \mu^{old}_{c(x)}, \Sigma^{old}_{c(x)})\, \pi^{old}_{c(x)}}{\sum_{n=1}^N \mathcal{N}(I(x) \mid \mu^{old}_n, \Sigma^{old}_n)\, \pi^{old}_n}$$

- M-step:

$$\pi_n = \frac{\sum_x p(c(x){=}n \mid I(x), \theta^{old})}{|\Omega|} \qquad \mu_n = \frac{\sum_x I(x)\, p(c(x){=}n \mid I(x), \theta^{old})}{\sum_x p(c(x){=}n \mid I(x), \theta^{old})} \qquad \Sigma_n = \frac{\sum_x (I(x) - \mu_n)(I(x) - \mu_n)^T p(c(x){=}n \mid \cdot)}{\sum_x p(c(x){=}n \mid \cdot)}$$

- Example: 5 components on a brain MR image; posterior maps $p(c{=}1\mid I), \dots, p(c{=}5 \mid I)$ shown.
- **Remarks**: quite robust; initialization matters if done badly; the **number of components** is important (may need prior knowledge of how many structures to extract); outputs are already useful (e.g., **gray-matter density maps**); used very regularly in famous tools — **SPM, FSL, FreeSurfer**; extensions to include better priors (e.g., an **atlas**) are possible.
- **Simple Segmentation Challenge** (Moodle): T1w and T2w volumes of the same individual. Goals: (1) perform bias removal on both and compare the bias fields — are they different? (2) perform EM segmentation on the individual images and jointly — does using multiple modalities give better-defined clusters?

## 8.2 Atlas-based segmentation

### The atlas concept

- Analogy: a **world atlas** — for each location it tells you what is there. A **brain atlas** does the same for anatomy (examples shown: T1-weighted, proton-density templates plus gray matter / white matter / CSF probability maps).
- An **atlas** adds prior information to segmentation — no longer simple clustering:
  - A **template in a reference frame**; for each location it provides information about what is there.
  - **Deterministic** (world atlas) or **probabilistic** — $p(c(x))$ (brain atlas).
  - Applicable to any anatomical structure.
  - Created by **averaging many different images** — the one shown was made from **152** different images (MNI152-style template).

### Formulation

- **Aligning to a reference frame**: register the atlas to the image (linear or non-linear registration); the image is mapped into the reference frame; at each point we now have a rough idea which structure to expect.
- Probabilistic formulation — start from the mixture model with unsupervised weights and **replace the mixture coefficients with the (registered) atlas prior**:

$$p(I(x) \mid \theta) = \sum_{n=1}^N \mathcal{N}(I(x) \mid \mu_n, \Sigma_n)\ p_{atlas}(c(x) = n), \qquad \theta = \{\mu_n, \Sigma_n\}_{n=1}^N$$

The atlas brings the prior information; the **posterior** now takes both atlas and intensities into account:

$$p(c(x) = n \mid I(x)) = \frac{\mathcal{N}(I(x) \mid \mu_n, \Sigma_n)\, p_{atlas}(c(x)=n)}{\sum_{n'} \mathcal{N}(I(x) \mid \mu_{n'}, \Sigma_{n'})\, p_{atlas}(c(x)=n')}$$

- Information propagates **from the atlas to the image**; no need to optimize the class probabilities $\pi_n$ anymore. *Do we still need to optimize $\mu_n, \Sigma_n$?* **Yes, for MRI** (no absolute intensity scale)! (For CT one may fix all parameters.)
- Comparison with/without atlas: **much better segmentation**; and there is **no randomness in the components** — we know what we ask for (component $n$ *is* e.g. gray matter).

### Analysis

- Used very commonly; easily generalizable to other body parts with an appropriate atlas; generalizable to other modalities (may fix all parameters for CT); an **outlier class** can be added to detect outliers.
- Requirements/caveats: need an atlas for each body part; the **atlas itself is very important** — may need atlases for different age groups, gender, race, …; **accuracy of the registration is very important**.
- **Two remedies** to improve robustness to atlas choice and registration: **multi-atlas segmentation** and **patch matching** (local searches in a sliding-window manner) — the latter covered in a later lecture.

### Multi-atlas segmentation

- Use **many atlases** rather than one: align all atlas images to the test image; **aggregate** the information from all of them; this remodels $p_{atlas}(c(x) = n)$.
- During aggregation, **weight atlases by registration quality**; example weighting schemes given:
  - $w_m \propto \|I(x) - A_m\|_2$ (intensity difference to atlas $m$)
  - $w_m \propto \|I(x) - T \circ A_m\|_2$ (difference to the *warped* atlas)
  - $w_m \propto |J_{T_{A_m \to I}}|$ (Jacobian determinant of the warp)
- Many atlases reduce the importance of any single one; with weighting, bad registrations are less likely to dominate.
- **Analysis**: solves some problems of single-atlas segmentation; **highly accurate**; was the **state of the art for a very long time**; leads to elegant probabilistic models [see the survey: *Multi-atlas segmentation of biomedical images*, J.E. Iglesias & M. Sabuncu, Medical Image Analysis, 2015]; but often requires **non-linear registration** and is **computationally VERY expensive** (many registrations).

## 8.3 Spatial constraints in segmentation

Motivation: so far **all pixels were treated independently** (atlases add some spatial information, but the final per-pixel decision still assumes independence). In real images this is highly unreasonable — **neighboring pixels are more likely to belong to the same object**. Two main approaches: (a) pre/post-processing (smoothing images beforehand; morphological operations); (b) **random field priors** (Markov random fields, conditional random fields).

### Morphological operations

- Segmentations suffer from **isolated islands** and **broken structures**, caused by noise, low-contrast boundaries, intensity overlap between structures, … Morphology operates on the segmentation (binary) image to **remove islands** and **fill gaps**.
- Properties of mathematical morphology operations: **shift-invariant**, **non-linear**, based on neighboring pixels defined through **structural elements**, applied in a sliding-window manner; binary images viewed as **sets of pixels**; defined for binary but extended to gray-level images.
- Two main operations ($A$ = image, $B$ = structural element, e.g. a small square of ones; $B_x$ = $B$ translated to $x$):
  - **Dilation**: $C = A \oplus B \triangleq \{x \mid B_x \cap A \ne \emptyset\}$
  - **Erosion**: $C = A \ominus B \triangleq \{x \mid B_x \subseteq A\}$
  - (Examples shown with $B = \mathbb{1}_{13\times13}$.)
- **Derived operations**:
  - **Opening**: $(A \ominus B) \oplus B$ — removes small islands/protrusions.
  - **Closing**: $(A \oplus B) \ominus B$ — fills small gaps/holes.

### Random field priors (MRF)

- Formulate **neighborhood consistency** in a probabilistic/energy model; main idea: **punish inconsistency** in the labeling of neighboring voxels, using the **Markovian property**.
- Previous models factorized fully: $p(I, c) = \prod_{x} p(I(x)\mid c(x))\, p(c(x))$ (independent pixels). To model connections between pixels, **change the prior**:

$$p(I, c) = p(c) \prod_{x\in\Omega} p(I(x) \mid c(x))$$

- **Markovian property**: with $G(x)$ = neighbors of $x$: $p(c(x) \mid c(/x)) = p(c(x) \mid c(G(x)))$ — the label at $x$ depends only on its immediate neighbors, given all others. The specific form depends on the neighborhood structure.
- **Energy formulation**: define an energy over neighboring label pairs,

$$E(c) = \sum_{x\in\Omega} \sum_{y \in G(x)} d(c(x), c(y))$$

with $d$ a distance between labels, and the corresponding **Gibbs distribution**

$$p(c) = \frac{1}{Z} \exp\{-E(c)\}$$

($Z$ = normalization constant). Lower distance → higher probability → enforces consistency.

- *Is this even allowed?* **Hammersley–Clifford theorem**: any probability distribution satisfying a Markovian property is a Gibbs distribution for an appropriate locally-defined energy, and vice-versa.

**Segmentation via posterior maximization**: the posterior $p(c \mid I) = p(I \mid c)p(c)/p(I)$ is now a **joint** distribution (no longer point-wise):

$$\arg\max_c p(c \mid I) = \arg\max_c\ \exp\{-E(c)\} \prod_{x\in\Omega} p(I(x) \mid c(x))$$

(assuming intensity at a voxel is independent of other voxels' intensities given its label). If the data model is also exponential, $p(I(x)\mid c(x)) \propto \exp\{-g(I(x)\mid\theta_{c(x)})\}$, this becomes **energy minimization**:

$$\arg\min_c\ \underbrace{\sum_{x\in\Omega} g(I(x) \mid \theta_{c(x)})}_{\text{unary term}} + \underbrace{\sum_{x\in\Omega}\sum_{y\in G(x)} d(c(x), c(y))}_{\text{pairwise term}}$$

- **Unary term** = data (fidelity) term — can also include any unary prior information on labels. **Pairwise term** = consistency term imposing similar labels on neighbors.
- If $c(x)$ is continuous (regression), usual optimization techniques apply; if **categorical** (segmentation), optimization is challenging.

**Simple example**: Gaussian observation model $p(I(x)\mid c(x)) = \mathcal{N}(I(x) \mid \mu_{c(x)}, \Sigma_{c(x)})$ so $g = (I(x)-\mu_{c(x)})^T \Sigma^{-1}_{c(x)} (I(x)-\mu_{c(x)})$; prior energy = **Ising/Potts model** $d(c(x), c(y)) = \lambda\, \delta(c(x) \ne c(y))$ (0 if labels equal, 1 otherwise):

$$\arg\min_c \sum_{x\in\Omega} (I(x)-\mu_{c(x)})^T \Sigma^{-1}_{c(x)} (I(x)-\mu_{c(x)}) + \lambda \sum_{x\in\Omega}\sum_{y\in G(x)} \delta(c(x) \ne c(y))$$

with $\lambda$ a trade-off between data fidelity and neighborhood consistency.

**Optimization**: computing the posterior is very difficult; exact optimization in 2D and higher is **NP-hard** [Boykov, Veksler, Zabih, TPAMI 2001]. Approximate energy minimization / posterior sampling methods: **Gibbs sampling** [Geman & Geman 1984], **iterated conditional modes (ICM)** [Ferrari et al. 1995], **graph cuts** [Boykov, Veksler, Zabih 2001], …

**Stochastic relaxation / Gibbs sampling — principle**:

- Iterative Monte-Carlo method. Sampling from the full posterior $p(c\mid I)$ is hard (normalization sums over all label configurations), but sampling from the **per-voxel conditional** is easy:

$$p(c(x) \mid c(/x), I) = \frac{p(I(x) \mid c(x))\ p(c(x) \mid c(G(x)))}{\sum_{c(x)} p(I(x) \mid c(x))\ p(c(x) \mid c(G(x)))}$$

- Start from a random/likelihood-based assignment $c_0$ (e.g., $\arg\max_c \prod_x p(I(x)\mid c(x))$); sample each voxel's posterior given the others; iterate over all voxels a number of times. Convergence properties in [Geman & Geman 1984].
- Example results: MRF smoothing with increasing $\lambda$.
- **Analysis of Gibbs sampling**: very simple to implement and effective; solution depends on parameters and initial conditions; may not converge to a pleasing result; convergence may be slow; **stochastic** — every run gives something different; other methods may do better; a complicated optimization problem overall.

### Conditional random fields (CRF)

- The MRF energy formulation used in a **discriminative** way: directly model $p(c(x) \mid I)$ without the Bayesian generative treatment:

$$\arg\min_c\ \underbrace{\sum_{x\in\Omega} g(c(x) \mid I)}_{\text{unary term}} + \underbrace{\sum_{x\in\Omega}\sum_{y\in G(x)} d(c(x), c(y))}_{\text{pairwise term}}$$

- Note the unary term now conditions on the whole image $I$.
- Can be used on its own (explicitly modeling $g$), or **with ML algorithm outputs** — e.g., the unary term can be the predictions of a **random forest**; commonly used to **clean up ML segmentation results**. Used regularly.

---
# 9. Lecture 7 — Statistical Shape Models & Active Shape Models (`Lecture_SSM+ASM.pdf`, 69 slides)

*Mauricio Reyes, Ph.D. (mauricio.reyes@med.unibe.ch), University of Bern*

**Lecture overview**: statistical shape modeling (quick summary/review); further considerations (reference selection, modeling scaling, outlier detection, modeling physiology); statistical shape modeling for segmentation: **Active Shape Models**.

## 9.1 Big picture — segmentation method families

| Simple methods | Classification + clustering | Deformable models | Active models |
|---|---|---|---|
| thresholding, region growing, … | kNN, SVM, … | snakes, level sets, … | **ASM, AAM** |

**Motivation for SSMs of human organs**: a standard **atlas** (think: a template) encodes prior knowledge; a **patient-specific SSM** balances *prior knowledge (what we know in advance)* vs. *observation (what we see)* — a statistical distribution of shape rather than a single template.

## 9.2 Mathematical tool — Principal Component Analysis (PCA)

- PCA **reduces the dimensionality** of a linear system: it projects the original data onto a lower-dimensional space, embedding the data in a more compact representation.
- 2D intuition: data points scattered along a principal direction; PCA yields the main (and secondary) directions and creates an **orthogonal coordinate system maximizing the variance** in each direction; points can be referenced along the main axis with a single parameter $b$ (2-D → 1-D).
- **SVD** diagonalizes the covariance matrix of the data: $M = U W V^T$ — $U$ contains the **eigenvectors** and $W$ the diagonal matrix of **eigenvalues** of $\mathrm{Cov}(M)$.
- Properties/caveats: PCA **assumes the data follow a Gaussian distribution** (even if in reality they do not); results in an orthogonal lower-dimensional system (principal axes perpendicular); PCA is a **linear** method. Other linear/non-linear dimensionality-reduction techniques exist: factor analysis, Isomap, multidimensional scaling (MDS), etc.

## 9.3 Building a statistical shape model

### Shape representation — point clouds

- Every surface point $p_i = \{x_i, y_i, z_i\}$; the surface is the concatenation $S = \{p_1, \dots, p_n\}$ with $n$ surface points; each shape becomes a vector
$$s_i = \{x_{i,1}, y_{i,1}, z_{i,1},\ x_{i,2}, y_{i,2}, z_{i,2}, \dots, x_{i,n}, y_{i,n}, z_{i,n}\} \in \mathbb{R}^{3n}$$

### Point-to-point correspondence

- For two objects of the same class (e.g., two femurs from different patients), isosurfacing initially yields point clouds with **different numbers of points** → one must establish **anatomical correspondence** between points.

### Point Distribution Models (PDM)

Prerequisites: a set of **aligned** shapes; each with the **same number** of surface points; points correspond anatomically and/or spatially; each shape a coordinate vector. Stack the $m$ shape vectors into a matrix $M = (s_1\ s_2\ \cdots\ s_m)$ (rows: all $x$-coordinates, then $y$, then $z$; $n$ = points per mesh, $m$ = training samples).

**Modeling shape variability**: subtract the mean from each instance, $D = M - \mathrm{mean}(M)$, and apply **SVD directly on $D$**: $D = U W V^T$, where $U$ = eigenvectors of $\mathrm{Cov}(M)$ and $\mathrm{diag}(W)$ = eigenvalues of $\mathrm{Cov}(M)$.

**Summary of statistical shape modeling**: the model consists of the **mean shape** $\bar x$ and the eigen-decomposition of the covariance $S\varphi_k = \lambda_k \varphi_k$ ($k$-th eigenvector/eigenvalue pair) — new shapes are generated as $x \approx \bar x + \Phi b$ where columns of $\Phi$ are eigenvectors ("modes of variation") and $b$ the shape parameters.

### Model evaluation — three standard measures

Used to indicate how well the modeling embedded the original data into a lower-dimensional space:

1. **Compactness** — how good the data reduction is: a compact model generates new shape instances with as few parameters (principal modes) as possible. Curve of cumulative explained variance vs. number of modes:
$$C(t) = \frac{\sum_{i \le t}\lambda_i}{\lambda_{total}}, \qquad \lambda_{total} = \text{sum of all eigenvalues}$$
2. **Generalization** — the ability to represent **new** instances of the class, measured by **leave-one-out** reconstruction: for every left-out instance build a model from the rest, compute its parameters $b_{out} = \Phi^{-1}(x_{out} - \bar x)$, reconstruct $\tilde x = \bar x + \Phi b_{out}$, measure error $\frac{1}{N}\sum_i (x_i - \tilde x_i)^2$ averaged over all training instances → the generalization curve (depends on number of modes used).
3. **Specificity** — how good the model is at generating instances **similar to the training set**: generate a large number of random instances with different numbers of modes, and for each compute the distance to the closest training shape.

## 9.4 SSM for segmentation — Active Shape Models (ASM)

[Cootes et al. 1995]

**General idea**:
1. Start with an SSM and an image; **initially place the SSM** in the image.
2. **Sample along the SSM surface normals**, find the largest gradient (= edge).
3. **Deform pose** (scale, rotation, translation) **and shape** (PCA modes) of the model to best fit the extracted image features.

### Initialization — a crucial step

Approximately align the SSM with the structure of interest. Many methods; choose per application: user interaction; centroid alignment; Chamfer matching; atlas registration; global search over the entire image (evolutionary algorithms). Illustration [Cootes et al., SPIE Medical Imaging 2001]: initial → 2 iterations → 20 iterations.

### Feature extraction, normals and profiles

- **Feature extraction**: edge detection methods (fill-in slide).
- **Normals**: the SSM's points + connections give a **mesh**, so each point's neighbors are known; the **cross product** of two (edge) vectors yields the orthogonal vector — the **surface normal**.
- **Sampling along profiles**: given a model point and its normal, sample along the normal profile; parameters: sampling range and step size → $2k + 1$ samples; **choose the point with the largest feature value (gradient)**; the model point should be transformed toward that point.

### Pose estimation

- Goal: rough alignment of both point sets; here a **2D similarity transform**:

$$T_{X_t, Y_t, s, \Theta}\begin{pmatrix}x\\y\end{pmatrix} = \begin{pmatrix}X_t\\Y_t\end{pmatrix} + \begin{pmatrix} s\cos\Theta & -s\sin\Theta \\ s\sin\Theta & s\cos\Theta \end{pmatrix}\begin{pmatrix}x\\y\end{pmatrix}$$

- Optimize the registration w.r.t. a cost function (e.g., mean Euclidean distance): minimize $|Y - T_{X_t,Y_t,s,\Theta}(x)|$ — solved e.g. via **ICP** (iterative closest point).

### Shape estimation

- **PCA-based deformation**: $b = P^T(x - \bar x)$, $x \approx \bar x + P b$; for target points $Y$: $b = P^T(Y - \bar x)$.
- Keep only the $k$ most important PCA modes (e.g., enough to account for **90% of variance**).
- **Threshold $b$** to allow plausible shapes only: $|b_i| < 3\sigma_i$ (3 standard deviations capture more than 95% of the variation of a Gaussian).

### Optimization scheme

- Pose and shape optimized separately or jointly; optimizer for pose (e.g., gradient descent), **closed-form solution for shape**.
- Iterative loop: compute profiles along normals → adjust pose → adjust shape → iterate until convergence. In formulas: $b = P^T\left(T^{-1}_{X_t,Y_t,s,\Theta}(Y) - \bar x\right)$, then the model instance in the image is $X = T_{X_t,Y_t,s,\Theta}(\bar x + Pb)$.

### Advanced methods

- **Multi-resolution pyramid**; handling noise by **averaging along points on the normal**; **intensity profiles** for modeling local structure:
  1. Sample along the profile for the $i$-th image; 2. avoid raw intensity changes → use **gradient values**; 3. **normalize**; 4. repeat for each model point; 5. assume a Gaussian distribution $(\bar g, S_g)$; 6. quality of fit of a new sample $s$ = **Mahalanobis distance** $f(s) = (s - \bar g)^T S_g^{-1}(s - \bar g)$; 7. pick the lowest value (highest probability).
- Examples: ASM in 2D [Cootes et al. SPIE 2001 — initial, 1 iteration, 14 iterations; clinical example]; ASM in 3D [Fripp 2007; Bauer 2012]; cranio-maxillofacial (CMF) surgery examples — ASM enhanced with **muscle information**; model enhanced with facial muscles, muscle fibers (anisotropy of tissue properties), predefined surgical approaches, data preprocessing (e.g., cropping), simulation-specific features (e.g., sliding contact areas).

### Statismo

- **Statismo** — open-source framework for PCA-based statistical models (http://statismo.github.com): provides a high-level API; users work with **shapes, not vectors**; core functionality implemented only once; **exchanging models becomes easy**; portable file format (HDF5); "representer" carries model semantics; supports sampling, probabilities, conditional distributions. Reference: Lüthi M., Blanc R., Albrecht T., Gass T., Goksel O., Büchler P., Kistler M., Bousleiman H., Reyes M., Cattin P., Vetter T., *Statismo — A framework for PCA based statistical models*, Insight Journal.

## 9.5 Active Appearance Models (AAM)

### Building texture models

- **Warp each training sample to the mean shape** to obtain a shape-free ("mean-free") patch; sample intensities from the shape-normalized image to form a **texture vector**; **normalize intensity** to eliminate lighting variations; then apply PCA on the normalized texture data (same procedure as for shape): $g \approx \bar g + P_g b_g$.

### Combined models of appearance

- Shape and texture are summarized by parameter vectors $b_s, b_g$; to handle **correlations** between them, stack $b = \begin{pmatrix} W_s b_s \\ b_g \end{pmatrix}$ (weighting $W_s$ accounts for unit differences between shape and texture), and apply **PCA on the combined $b$**: $b = P_c c$ — $c$ is the vector of **appearance parameters controlling both shape and texture**.
- Linear model: shape and texture expressed directly as functions of $c$:

$$x = \bar x + Q_s c, \qquad g = \bar g + Q_g c, \qquad Q_s = P_s W_s^{-1} P_{cs}, \quad Q_g = P_g P_{cg}$$

- **Generating a new image**: generate the shape-free intensity image from $g$, then warp it using the control points described by $x$.

### AAM model fitting

- Minimize the difference between the new image and the **synthetic AAM image**; key step: estimate the parameter update from the current image sample; difference vector $\delta I = I_i - I_m$ (image vs. model).
- Details: residual $r(p) = g_s - g_m$ with parameter vector $p = (c, t, u)$ (appearance, pose, texture normalization); error $E(p) = |r(p)|^2$; first-order Taylor expansion $r(p + \delta p) = r(p) + \frac{\partial r}{\partial p}\delta p$; choosing $\delta p$ to minimize $E(p + \delta p)$ gives

$$\delta p = -R\, r(p), \qquad R = \left(\frac{\partial r}{\partial p}^T \frac{\partial r}{\partial p}\right)^{-1} \frac{\partial r}{\partial p}^T$$

i.e., the pseudo-inverse of the **Jacobian** $J = \partial r/\partial p$, which is **estimated by applying small displacements on the training set** (precomputed).

- Iterative algorithm: measure residual $r(p)$ → predict correction $\delta p = -R\,r(p)$ → update $p = p + k\,\delta p$ → repeat until convergence.
- Examples: AAM face model fitting [Cootes 2004; Dornaika 2003]; **face synthesis** with parameters pose, shape, illumination [Blanz & Vetter 1999 — 3D morphable models].

### Comparison ASM vs. AAM

| Evaluation | Differences |
|---|---|
| ASM usually has a **larger capture range** | ASM searches **around** the current position along normals; AAM searches **at** the current position |
| ASM is **faster** | ASM minimizes **distance between model and corresponding points**; AAM minimizes **difference between synthetic and target image** |
| ASM usually has more accurate feature-point location | |
| AAM gives a **better match to image texture** | |

## 9.6 Conditional shape models and applications

- **Conditional shape models**: predict the **full shape from surrogate variables** (lengths, angles, surface points, etc.).
- **COSMOS — Corrections on Multi-Organ Segmentations** [Valenzuela et al. 2016]: aim — **"modify one, correct many!"**; single-organ correction approaches create inconsistencies with neighboring organs; solution: **conditional statistical shape models** via covariance-matrix decomposition → a **posterior shape model given a partially or completely corrected organ** (using submatrices $\Sigma_{gg}$, $\mu_g$ of the joint covariance/mean).
- **Combining shape models with supervised learning** for bone shape and cortical thickness estimation.
- **Image-guided soft-tissue deformation for CMF surgery**: pipeline — facial landmark tracking and pose estimation with a 3D active shape model → extract silhouette information → final shape modeling → texture mapping; adding silhouette information reduces reconstruction error (mm) as the number of silhouette frames increases (2–8 frames, two cases shown).
- **Computer-assisted aesthetic face surgery**: web-based, improvements over time, data mining, iPad app.

## 9.7 References

1. Cootes T.F., Taylor C.J., Cooper D.H., Graham J., "Active Shape Models — Their training and application," *Computer Vision and Image Understanding*, 1995.
2. Cootes T., Taylor C.J., "Statistical Models of Appearance for Computer Vision," Tech Report, University of Manchester, 2004.
3. Heimann T., Meinzer H., "Statistical Shape Models for 3D Medical Image Segmentation: A Review," *Medical Image Analysis*, 2009.

---
# 10. Lecture 8 — (Multi-)Atlas & Patch-Based Segmentation (`AtlasPatch_segmentation.pdf`, 52 slides)

*Mauricio Reyes, PhD, University of Bern — ARTORG Center for Biomedical Engineering, Medical Image Analysis Group. Slide credits: D. Rueckert, L. Wang, T. Tong.*

**Table of contents**: atlas-based segmentation · multi-atlas-based segmentation · patch-based segmentation.

## 10.1 Atlas-based segmentation (segmentation using registration)

- Idea: an **atlas** (image + manual segmentation) is registered **atlas-to-image** to an unseen patient image; the atlas labels are propagated → automatic segmentation.
- **Factors influencing the quality of the result**: image similarity; presence of pathology; quality of the registration; likelihood of the patient image (how typical it is of the atlas population).
- **Bias in reference selection** [Rueckert et al. MICCAI 2001; mean-image computation: Guimond et al. CVIU/MICCAI 2000]: results built with different references are "**Not the same!**" — choosing a different reference subject yields different average models (average model using ref 1 vs. average intensity using ref 2) → motivates **natural coordinate references** and **group-wise modeling** (computing an unbiased mean image).

## 10.2 Multi-atlas-based segmentation

- Example atlas database [A. Hammers et al., *Three-dimensional maximum probability atlas of the human brain, with particular reference to the temporal lobe*, Human Brain Mapping 19(4):224–247, 2003]: T1-weighted MR from **30 volunteers** (ages 20–54, median 30.5; 15 male, 15 female); 256×256×124 volumes (resolution 1.25 × 0.94 × 1.50 mm); each image manually segmented into **83 anatomical structures**.
- **Multi-atlas segmentation using classifier fusion** [Heckemann et al., NeuroImage 2006]: register every atlas to the unseen data → each atlas produces an individual segmentation → **decision fusion** → final segmentation.

### How do you fuse?

- **Global fusion strategies**: majority voting; weighted voting.
- **Local fusion strategies**: locally weighted fusion; **STAPLE** (Simultaneous Truth And Performance Level Estimation).
- **Shape-based averaging**.

Details:

- **Simple majority voting** (illustrated with 5 segmentations of a voxel: hippocampus ×2, background ×3 → background wins).
- **Shape-based averaging (SBA)**: average the *shapes* of the segmentations, based on averaging the **distance transforms** of the segmentations (VOTE vs. SBA comparison shown).
- **STAPLE** — probabilistic approach: *what is the most probable underlying true segmentation given the observed segmentations?* Simultaneously estimates the **performance of each rater/segmentation**: sensitivity $p$ (true-positive fraction) and specificity $q$ (true-negative fraction). Setup: $D$ = data (expert segmentations to merge) known; $T$ = ground truth unknown; $(p, q)$ per segmentation unknown. Find $(p, q)$ and $T$ **maximizing the likelihood** of observing $D$. Solved iteratively by **Expectation-Maximization** [Warfield et al., 2004]: initialize $p^{(0)}, q^{(0)}$ → E-step: compute $f(T \mid D, p^{(0)}, q^{(0)})$ → M-step: use the current ground-truth estimate to compute the next $p^{(1)}, q^{(1)}$ → iterate.

### Multi-atlas segmentation with classifier **selection** and fusion

[Aljabar et al., NeuroImage 2009]

- Pipeline: affine registration of atlases to a standard space → **selection of similar atlases** for the unseen data (discard dissimilar ones) → non-rigid registration of selected atlases to the unseen data → individual segmentations → decision fusion.
- Result: **classifier selection & fusion outperforms naive fusion of all atlases** (accuracy vs. number of atlases plot).

## 10.3 Patch-based segmentation

- Patch-based methods have been popular in computer vision: texture synthesis (Efros & Freeman 2001), in-painting (Criminisi et al. 2004), restoration (Buades et al. 2005), single-frame super-resolution (Protter et al. 2009).
- **Basic idea**: only **coarse registration** required (rigid or affine); then use the same concept as label fusion, but at **patch level**.

### From patch-based denoising to label fusion

- **Non-local means** [Buades et al. 2005]: neighborhood averaging assuming **self-similarity** — every patch in a natural image has many similar patches in the same image; weights from patch similarity with an exponential weighting function $\exp(-x)$ and a **bandwidth** parameter controlling smoothing.
- **Patch-based label fusion**: apply the same idea to label fusion — proposed **simultaneously by Rousseau et al. and Coupé et al. in 2011**. Each target-image patch is compared to patches in the (coarsely aligned) atlas library within a search window; atlas labels are propagated with NLM-style similarity weights.

### Application: patch-based multi-organ segmentation of abdominal CT

[Wolz et al., MICCAI 2013; IEEE-TMI 2013] — CT scans from **150 subjects**; **hierarchical patch-based label fusion**. Results (Dice / Jaccard, mean (std) [range]):

| Structure | Dice | Jaccard |
|---|---|---|
| Kidneys | 92.5 (7.2) [51.5–98.2] | 86.8 (10.5) [34.6–96.4] |
| Liver | 94.0 (2.8) [81.4–97.3] | 88.9 (4.8) [68.7–94.9] |
| Pancreas | 69.6 (16.7) [6.9–90.0] | 55.5 (17.1) [3.6–83.3] |
| Spleen | 92.0 (9.2) [26.4–98.2] | 86.2 (12.7) [15.2–96.4] |

### Application: patch-based segmentation of brain structures (neonatal tissue)

[Wang et al., NeuroImage 2014]

- Pipeline: from template images build a **subject-specific atlas** for the testing subject (patch-based, sparse representation), impose **local spatial consistency**, then **level-set segmentation**; iterated in steps (Step 1, 2, 3, …).
- Comparison of priors: population-based atlas vs. subject-specific atlas vs. subject-specific atlas **with spatial consistency**, for WM/GM/CSF on an original T2 image — subject-specific + consistency is sharpest.
- Sparse patch representation (elastic-net problem): each target patch $X$ is coded over a dictionary $D = [\text{WM}\ \text{GM}\ \text{CSF}]$ of template patches:

$$\min_{\alpha \ge 0}\ \tfrac{1}{2}\|X - D\alpha\|_2^2 + \lambda_1 \|\alpha\|_1 + \tfrac{\lambda_2}{2} \|\alpha\|_2^2$$

with sparse non-negative coefficients $\alpha$ → Step 1: subject-specific atlas.

- **How many templates are needed?** Box-whisker plots of Dice vs. number of templates (leave-one-out with a library of 20 templates) — performance saturates as templates increase.
- **Leave-one-out cross-validation on 20 subjects** (WM Dice per subject, ~0.65–0.93): compared methods — **MV** (majority voting), **CLS** (coupled level sets; Wang L. et al., NeuroImage 2011), **CPM** (conventional patch-based method; Coupé et al., NeuroImage 2011), subject-specific atlas, proposed without spatial consistency, **proposed with spatial consistency (best)**.
- **8 testing subjects with manual segmentations**: Dice for WM and GM (~0.8–0.94), proposed > CPM > CLS; difference maps shown. **94 testing subjects** for qualitative evaluation — proposed gives visually cleanest results.

### Patch-based labeling vs. Sparse Representation Classification (SRC) vs. Discriminative Dictionary Learning for Segmentation (DDLS)

[Tong et al., NeuroImage 2013]

- **SRC**: assumes a target patch can be represented by a **few representative patches** from the patch library $p_i$ with sparse coefficients $a_i$ — solved as an **elastic-net problem**; labels follow from which library patches are selected.
- **DDLS**: adds a **learned linear classifier** to dictionary learning — jointly groups patch and label information and imposes sparsity. Benefits: **decreases computational burden via a smaller dictionary**; **better exploits the discriminative power of the patch library**. Implementation: normalized **voxel-wise dictionaries and classifiers**; final output $\{D, W\}$ (dictionary + classifier); example dictionary shown with 16×16 atoms of size $5^3$.
- **Three steps of DDLS**: (1) extract patch $p$; (2) solve the sparse coding problem for coefficients; (3) **label the voxel using the classifier** ($h_t$ = class label vector).
- Qualitative: manual vs. automatic segmentations (hippocampus). Quantitative: comparison on the **ADNI dataset** using Dice — patch-based vs. SRC vs. **DDLS (best)**.

---
# 11. Interactive Demo — (Multi)Atlas & Patch-Based Segmentation (`atlas_patch_segmentation_demo.html`)

*Companion artifact to Lecture 8 — "(Multi)Atlas & Patch-Based Segmentation — Interactive Demo", footer credit: M. Reyes · University of Bern & ETHZ.*

A **self-contained single-file web app** (React 18 + Babel standalone from CDNs, dark GitHub-style theme, IBM Plex fonts) that lets you play with the three ideas of the lecture on **procedurally generated 2D brain slices**. Technical contents:

## 11.1 Synthetic data generator

- Deterministic pseudo-random pipeline: `mulberry32` seeded RNG → `noise2D` → bilinearly-smoothed `smoothNoise` → **fractional Brownian motion** `fbm` (4 octaves, amplitude halved and frequency doubled per octave).
- `generateRealisticBrain(W, H, params)` draws a parametric axial brain slice; tunable anatomy parameters include: skull ellipse semi-axes (0.44/0.50), cortex thickness base (0.11), cortical folding frequency/amplitude (7 / 0.035), ventricle scales/offsets/angle, thalamus size (0.07), caudate size (0.04), tissue intensities (WM 0.78, GM 0.52, CSF 0.22), and noise level (0.04). Varying the seed/parameters produces an **atlas library** of distinct subjects plus a **target** subject, each with ground-truth label maps (background + 3 tissue classes; per-class colors and names, Dice bars rendered per class).
- Utility components: `BrainCanvas` (renders intensity/label/mismatch modes, patch highlight box), `HeatmapCanvas` (normalized weight heatmaps), `DiceBar`, `InfoBox`, `SectionLabel`, `MiniPatch`; `computeDice(labelsA, labelsB, classId)` implements the standard Dice overlap per class.

## 11.2 Tab 1 — "Single atlas segmentation" (`AtlasTab`)

- Pick one atlas from the library; its (registered) label map is propagated to the target; the app shows the target, the propagated segmentation, and a **mismatch view**, plus **Dice scores per class (CSF/GM/WM) and their average** for "Atlas *k* → Target".
- Demonstrates the lecture point that single-atlas quality depends on how similar the chosen atlas is to the patient.

## 11.3 Tab 2 — "Multi-Atlas Fusion" (`FusionTab`)

- Adjustable number of atlases; three fusion methods with in-app descriptions (quoted from the demo):
  - **Majority Voting** — "Each atlas gets equal weight (w=1). The most common label wins. Simple but ignores atlas quality."
  - **Global Weighted Voting** — "Each atlas gets one fixed weight based on overall image similarity to the target. The same weight applies at every voxel." Implemented as $w_a = \exp(-\mathrm{SSD}_a / (count \cdot 0.04))$ computed once over the whole image.
  - **Locally Weighted Voting** — "Each atlas's weight varies per voxel based on local patch similarity (7×7 neighborhood). Better atlases in a region get more influence there." Implemented as $w_a(x) = \exp(-\mathrm{localSSD} / (count \cdot 0.03))$ over a 7×7 patch.
- The explanatory text notes the key question of *how to weight each atlas's contribution* — equally, by global image similarity (one weight per atlas), or by local patch similarity ("an atlas that matches well near the ventricles may match poorly in the cortex").
- **Voxel inspector**: click on the fused result to see, for that voxel, every atlas's weight as a bar (green when the atlas's label equals the true label, red otherwise) with the numeric weight; the caption clarifies for each mode ("All weights = 1. Pure vote counting." / "Global weights — computed once from whole-image similarity." / "Local weights — computed from a 7×7 patch around this voxel."). Dice bars per class update live ("Dice — N atlases, ⟨method⟩").

## 11.4 Tab 3 — "Patch-Based NLM" (`PatchTab`)

- Interactive **non-local-means label fusion** at a clicked target voxel ("Target (click to select)"). Sliders: **patch size** (default 5×5), **search radius** (2–15, default 5), and **β** bandwidth ("Low β → few patches dominate · High β → many contribute"); fixed $\sigma^2 = 0.008$.
- Computation (as implemented): extract the target patch around the selected voxel; for every atlas and every location in the search window, compute the patch SSD and the NLM weight

$$w = \exp\left(-\frac{\|P_{target} - P_{atlas}\|_2^2}{2\, N\, \beta\, \sigma^2}\right)$$

($N$ = number of pixels in the patch). Matches are ranked by weight; per-label scores accumulate the weights and the label decision is displayed with the formula shown in the UI:

$$L(x) = \arg\max_l\ \frac{\sum_i w_i\, \delta(\text{label}_i = l)}{\sum_i w_i}$$

- Displays: the top-matching mini-patches with their weights/labels, the resulting per-class probabilities, correctness indicator (✓/✗ vs. the true label), and **"NLM Weight Heatmaps per Atlas"** showing where in each atlas's search window the weight mass concentrates.

The demo thus operationalizes exactly the lecture concepts: single-atlas propagation → fusion strategies (majority/global/local) → patch-based NLM label fusion with patch size, search window, and bandwidth as the governing parameters.

---
# 12. Lecture 9 — Convolutional Neural Networks (`Lecture09_CNN.pdf`, 72 slides)

*Ertunc Erdil (slide date printed: April 2025)*

## 12.1 Introduction to neural networks

- Supervised learning setting: an unknown true function $y = f^*(x)$ is approximated by a parametric model $g(x; \theta)$, trained by

$$\theta^* = \arg\min_\theta\ \mathcal{L}(y,\ g(x;\theta)) + R(\theta)$$

- Deep networks are **compositions of layers**: $g = g_L \circ \cdots \circ g_2 \circ g_1(x)$.
- **Perceptron** (single layer): $g(x;\theta) = g_1(x; W, b) = \sigma(Wx + b)$.
- **Multi-layer perceptron (MLP)**: $h^l = \sigma_l(W_l h^{l-1} + b_l)$, $h^l \in \mathbb{R}^{d_l \times 1}$, with $h^0 = x$ and $h^L = g(x;\theta)$; for binary classification $\sigma_L$ = **sigmoid**; for multi-class $\sigma_L$ = **softmax**. (A slide surveys common activation functions.)
- This lecture: **CNNs** — a single layer replaces the matrix product by convolution:

$$\text{MLP: } h^l = \sigma_l(W_l h^{l-1} + b_l) \qquad \text{CNN: } h^l = \sigma_l(W_l * h^{l-1} + b_l)$$

**Outline**: motivation · convolutional layers · strides · a simple image-classification CNN · pooling.

## 12.2 Motivation — problems of MLPs on grid-like data

Feeding a **vectorized image** into an MLP raises **three issues** with the linear transformation $W_l h^{l-1}$:

1. **Fully connected links lead to too many parameters.** Example: $x \in \mathbb{R}^{N_0 \times M_0}$ with $(N_0, M_0) = (64, 64)$ and $d_1 = 128$ hidden units → $W_1 \in \mathbb{R}^{d_1 \times (N_0 M_0)}$ = $64 \times 64 \times 128 = 524{,}288$ parameters (without biases) — and this even *compresses* from 4096 to 128 dimensions, whereas (illustrated with the $d_1 = 1, 2, 3$ toy example) we actually want to **lift the data to a higher dimension**.
2. **Images are composed of a hierarchy of local statistics.**
3. **Lack of translation invariance.**

Applications shown: **image classification** — diabetic retinopathy grading (Healthy / Mild / Moderate / Severe / Proliferative) [Pratt et al., *Procedia Computer Science* 2016]; **age regression** from images (17 / 30 / 74 / 81 years old); **image segmentation** — brain tumor segmentation [Menze et al., "The multimodal brain tumor image segmentation benchmark (BRATS)", *IEEE TMI* 2014].

## 12.3 Convolutional layers

### The convolution operator

- 1D, two functions $f(t), g(t)$: continuous $f * g(t) = \int f(\tau) g(t - \tau)\, d\tau$; discrete $f*g(t) = \sum_{\tau=-\infty}^{+\infty} f(\tau) g(t-\tau)$. In words: the integral of the product of two functions **after one is reversed and shifted**.
- 2D, image $I(x,y)$ and kernel $k(x,y)$: $I * k(x,y) = \sum_m \sum_n I(m,n)\, k(x-m, y-n)$.
- Convolution is **commutative**: $I * k = k * I = \sum_m\sum_n I(x-m, y-n) k(m,n)$; commutativity is *the only reason* one reverses a function before shifting — in practice not very useful.
- **Convolution vs. cross-correlation**: cross-correlation is $\sum_m \sum_n I(x+m, y+n)\, k(m,n)$ (no flip). **In practice, many ML libraries implement cross-correlation under the name "convolution."** Kernels are also called convolutional *weights* or *filters*. (Sliding-kernel visualization over an image with a 3×3 kernel `a…j`.)

### Convolutions instead of projections

Fully connected activation vs. convolutional activation:

$$a_{l,k} = \sum_j w_{l,kj}\, h_{l-1,j} + b_{l,k} \qquad\longrightarrow\qquad a_{l,k} = \sum_j w_{l,kj} * h_{l-1,j} + b_{l,k}$$

- FC: each $h_{l-1,j}$ is a *number* (one neuron); separate weight links every neuron to every activation; high dimensions → huge weight counts.
- CNN: each $h_{l-1,j}$ is an **image of neurons** — the $j$-th **channel** at layer $l-1$; a separate **convolutional kernel** $w_{l,kj}$ links channel $j$ to output channel $k$, and the **same kernel applies to the entire image of neurons** (weight sharing). Nonlinearities after activations remain as in MLPs. A "vector of neurons" (FC layer) becomes an $N_l \times M_l$ **channel**, and a layer has $d_l$ channels.

### Layer sizes and parameter counts

- First layer, input $x \in \mathbb{R}^{3\times N_0 \times M_0}$:
  - Fully connected: $W_1 \in \mathbb{R}^{d_1 \times (3 N_0 M_0)}$, output $h^1 \in \mathbb{R}^{d_1}$.
  - Convolutional: kernels $w_{1,j} \in \mathbb{R}^{3 \times k_1 \times k_2}$, $W_1 \in \mathbb{R}^{d_1 \times 3 \times k_1 \times k_2}$, output $h^1 \in \mathbb{R}^{d_1 \times N_1 \times M_1}$.
- Intermediate layer: FC $W_l \in \mathbb{R}^{d_l \times d_{l-1}}$; convolutional $W_l \in \mathbb{R}^{d_l \times d_{l-1} \times k_1 \times k_2}$ mapping $h^{l-1} \in \mathbb{R}^{d_{l-1}\times N_{l-1} \times M_{l-1}} \to h^l \in \mathbb{R}^{d_l \times N_l \times M_l}$.
- Takeaway (stated twice): **"Larger layers with sparser connections with lower number of parameters."**
- **Local vs. global information gathering**: an MLP must represent the entire image in a single vector (needs large $d_1$); a CNN represents **local neighborhoods** in vectors — no need for large $d_1$ to retain information.

### Boundary handling — valid vs. same convolution

When the kernel sits inside the image there's no problem; at the boundary, out-of-bounds values are undefined. Two options:

1. **Valid convolution** — evaluate only where all elements are defined; the kernel is evaluated within the interior area; you **lose pixels at each end**: $N_l = N_{l-1} - k_1 + 1$, $M_l = M_{l-1} - k_2 + 1$.
2. **Same convolution (padding)** — pad the boundaries so the output has the same size: pad to $(N_{l-1}+k_1-1)\times(M_{l-1}+k_2-1)$ so $N_l = N_{l-1}, M_l = M_{l-1}$. The pad **value** is a parameter — zero padding is common, but symmetric padding also exists.

**Exercise slide** (parameter counting): a classification network with 16 output classes; compare $x \in \mathbb{R}^{64\times64} \to 256 \to 128 \to 64 \to 16$ where all links are fully connected (one bias per layer) versus the same architecture where the first three links are **valid 5×5 convolutions** (one bias per channel, arrows FC) — count parameters and hidden neurons per layer.

### Receptive fields and feature hierarchies

- Convolutional layers **hierarchically aggregate local spatial features** — as layers progress, more global information is encoded; the network extracts task-specific features (receptive field grows with depth).
- Simplified example: layer 1 = derivative filters followed by ReLU; layer 2 channels = weighted sums of 2nd-order derivatives.

### Translation invariance and equivariance

- Translation invariance is **not native to fully connected networks**: a translated object produces very different hidden activations, and the rest of the network sees different activations. In many vision applications translated inputs should give identical outputs.
- Quiz: applications where two translated images should give (1) *identical outputs*: **recognition**; (2) *different outputs*: **detection, localization, segmentation** — though for these we want the *same output at a different location*.
- Could you teach an FC network invariance? Yes, by applying **random translations** (augmentation) of each training image — "not the most elegant way!"
- **Convolution helps** — it is **translation equivariant**: applying a transformation to the input yields the same result as applying it to the output, $f(T(x)) = T(f(x))$. Convolution is *equivariant*, but **not invariant**: $f(T(x)) \ne f(x)$.

## 12.4 Strides

- Problem: with 5×5 kernels and valid convolutions, going from 128×128 down to a 1×1 output (a recognition system with #channels = #classes) shrinks only by 4 pixels per layer (128→124→120→116→…) — you would need **many layers or very large kernels**.
- **Strided convolution**: instead of moving the kernel one pixel at a time, skip $s$ pixels. Channel size becomes

$$N_l = \left\lceil \frac{N_{l-1} - k_1}{s_1} \right\rceil, \qquad M_l = \left\lceil \frac{M_{l-1} - k_2}{s_2} \right\rceil$$

- With 5×5 kernels, valid padding and stride 2: 128×128 → 62×62 → 29×29 → 12×12 — the dimension **drops very quickly**; higher stride → faster drop.
- Caveats: **you lose information** (more with higher stride); **you do not gain translation invariance**; if used, stride 2 in all directions is most common.
- Summary: strides help with dimensionality reduction but not much with translation invariance.

## 12.5 Pooling

- **Pool information in a neighborhood**: represent the region with one number (summarize); applied to **each channel separately**.
- Variants: **max-pooling** (maximum activation — most common), **min-pooling** (minimum), both non-linear (like median filtering); **average pooling** is a linear operator.
- **Max pooling** properties:
  - Represents the entire region by the neuron with the highest activation.
  - **Partial local translation invariance**: (worked example with a 2×2 pooled block) the max value 617 can be at any neuron within the highlighted area and the pooled value will not change. It does **not** give complete translation invariance.
  - Often applied with **stride equal to the pooling kernel size**; the pooling kernel has **no learnable parameters**.
  - **Substantial dimensionality reduction**: even a 2×2 pooling kernel halves the image side; larger kernels reduce more. It is a **non-linear** dimensionality reduction — only the most prominent activation is transmitted to the next layer.
  - More advanced pooling mechanisms exist, e.g., **CapsuleNets** [Sabour, Frosst and Hinton 2017].
- Scoreboard: with convolution + pooling, the three MLP issues are addressed — parameters (solved by weight sharing), hierarchy of local statistics (solved by stacked convolutions), translation invariance (**partially solved** by pooling).

## 12.6 A simple image classification CNN

Architecture pattern:

> [Convolution → non-linearity → max-pooling] × repeated → fully connected layer(s) (transformation → non-linearity) → **output layer with #neurons = #classes**.

- Local features are extracted and aggregated throughout the network; the **last layers "see" the entire image** and their features encode global information.
- The final convolutional channel must see large areas of the image so semantic labels can be determined — due to preceding convolutions and poolings, each neuron there has a **large receptive field**.

---
# 13. Lecture 10 — Attention and Transformers (`Lecture10_Transformers.pdf`, 51 slides)

*Ertunc Erdil, April 2026*

**Framing**: attention is the fundamental block of Transformers. Historical context slides (credit: Lucas Beyer): the classical landscape had **one architecture per "community"** (CNNs for vision [Aphex34/Wikipedia figure], RNN/LSTM for sequences [GChe/Wikipedia figure]); the **Transformer "took over" one community at a time** [architecture figure from the "Attention is All You Need" paper].

**Outline**: motivation · self-attention · scaled self-attention · transformer layers · multi-head self-attention · positional encoding · transformers for images.

## 13.1 Motivation

Two example sentences about the word **"bank"**:

- *"I swam across the river to get to the other side of the bank"*
- *"I walk across the road to get cash from the bank"*

**Some words are more important than others** for disambiguating meaning. A standard network (an MLP $h = \sigma(Wx + b)$ applied per word) processes each token independently. **Attention** instead builds the representation of "bank" as a weighted combination of context words: $a_1 \cdot \text{cash} + a_2 \cdot \text{bank}$, or $a_1 \cdot \text{swam} + a_2 \cdot \text{river} + a_3 \cdot \text{bank}$.

## 13.2 Self-attention (built up in stages)

Input tokens/embeddings $x_1, \dots, x_N$, $x_i \in \mathbb{R}^{D\times1}$; output tokens $y_i$. **Attention should be a function of all tokens** $x_1, \dots, x_N$:

$$y_i = \mathrm{Attention}(x_1, \dots, x_i, \dots, x_N) = \sum_{j=1}^{N} a_{ij}\, x_j, \qquad a_{ij} \ge 0, \quad \sum_{j=1}^N a_{ij} = 1$$

**Stage 1 — parameter-free dot-product attention**:

$$a_{ij} = \frac{\exp(x_i^T x_j)}{\sum_{j'=1}^{N} \exp(x_i^T x_{j'})} \qquad \Longleftrightarrow \qquad Y = \mathrm{Softmax}(XX^T)\, X$$

with $X \in \mathbb{R}^{N \times D}$ ($N$ tokens × $D$ features); **softmax is applied to each row separately** (emphasized repeatedly).

*Issues*: (1) **no capacity to learn from data**; (2) each feature value within a token plays an **equal role** in determining attention coefficients.

**Stage 2 — add learnable parameters**: transform $\tilde X = XU$, $U \in \mathbb{R}^{D\times D}$:

$$Y = \mathrm{Softmax}(\tilde X \tilde X^T)\, \tilde X = \mathrm{Softmax}(XUU^TX^T)\, XU$$

Learnable parameters help select more useful features. *Remaining issue*: $XUU^TX^T$ is **symmetric** — but real attention relationships are not: in the sentence example, "swam" should connect strongly to "bank" (river bank) while the reverse connection may be weak.

**Stage 3 — different learnable parameters for Key, Query, Value**:

$$K = XW_K\ (W_K \in \mathbb{R}^{D\times D_K}), \quad Q = XW_Q\ (W_Q \in \mathbb{R}^{D\times D_Q}), \quad V = XW_V\ (W_V \in \mathbb{R}^{D\times D_V})$$

$$Y = \mathrm{Softmax}(QK^T)\, V$$

Now $QK^T$ is **asymmetric**.

**Key/Query/Value terminology from information retrieval** — movie-database example: keys = the catalog entries (Titanic: romance/drama, 1997, DiCaprio & Winslet; Inception: sci-fi/thriller, 2010, DiCaprio; Black Swan: drama/thriller, 2010, Portman; Mad Max: action/adventure, 2015, Hardy & Theron; The Grand Budapest Hotel: comedy/drama, 2014, Fiennes); query = "genre: sci-fi, year: after 2000s, leading actor: Leonardo DiCaprio"; values = the movie files themselves (`Titanic.avi`, `Inception.avi`, …). Classic retrieval does **hard attention** (select the single most similar movie); transformers **generalize to soft attention**, using continuous variables to measure the degree of match between query and keys.

## 13.3 Scaled self-attention

$$Y = \mathrm{Softmax}\!\left(\frac{QK^T}{\sqrt{D_K}}\right) V$$

- Motivation: the gradients of softmax become **exponentially small for very small or large inputs** (saturation).
- Justification of the scale: if $Q, K \sim \mathcal{N}(0, I)$ then $\mathrm{Var}(QK^T) = D_K$ — dividing by $\sqrt{D_K}$ normalizes the variance.

## 13.4 Multi-head self-attention

- Analogy to **multiple channels/filters in CNNs**: there may be *different useful features to extract at each layer* → CNNs use multiple filters; likewise **there might be multiple patterns that require attention → use multiple attention heads**.
- Per head $h \in [1, H]$: $K_h = XW_{K_h}$, $Q_h = XW_{Q_h}$, $V_h = XW_{V_h}$;

$$H_h = \mathrm{Attention}(Q_h, K_h, V_h) = \mathrm{Softmax}\!\left(\frac{Q_h K_h^T}{\sqrt{D_{K_h}}}\right) V_h$$

- Combine heads: $Y = \mathrm{Concat}(H_1, \dots, H_H)\, W_0$ with $\mathrm{Concat}(\cdot) \in \mathbb{R}^{N \times H D_V}$ and $W_0 \in \mathbb{R}^{HD_V \times D}$. [Figure credit: *Deep Learning: Foundations and Concepts*, C. Bishop & H. Bishop.]

## 13.5 Transformer layer

Block structure (input $X \in \mathbb{R}^{N\times D}$ → output $Y \in \mathbb{R}^{N\times D}$):

> $X$ → **multi-head self-attention** → **(+) residual connection** → **LayerNorm** → **MLP (shared for each token)** → **(+) residual connection** → **LayerNorm** → $Y$

(i.e., two sub-blocks — attention and per-token MLP — each with a residual connection and layer normalization.)

## 13.6 Positional encoding

- **Transformers are equivariant with respect to input permutations** (compare: CNNs are translation equivariant, $f(P(X)) = P(f(X))$). Consequence: *"I bought an apple watch"* and *"watch an apple I bought"* are **the same for a transformer**!
- Worked example: computing attention for token $q_4$ ("apple") against keys $k_1 \dots k_5$ gives scores $a_{4,1} \dots a_{4,5}$, scaled softmax $a'_{4,j}$, output $\mathrm{Attention}(q_4, K, V) = \sum_j a'_{4,j} v_j$ — a **sum**, identical for both word orders → *"it is impossible to understand the meaning of the word with only attention."*
- **Solution: encode token order in the data** — add a positional vector: $\tilde x_i = x_i + r_i$, with $r_i, x_i \in \mathbb{R}^{D\times1}$.
- An **ideal positional encoding** should: provide a unique representation for each position; be bounded; generalize to longer sequences; encode the *relative* positions of tokens.
- **Sinusoidal encoding** (position $n$, dimension $i$, base $L$):

$$r_{ni} = \begin{cases} \sin\left(\dfrac{n}{L^{i/D}}\right) & i \text{ even} \\[2mm] \cos\left(\dfrac{n}{L^{(i-1)/D}}\right) & i \text{ odd} \end{cases}$$

Visualizations for $D = 6, L = 30, N = 200$ and $D = 100, L = 30, N = 200$ [figure: Bishop & Bishop].

- With positional encoding, the full pipeline is: $X$ + positional encoding → transformer layer (multi-head self-attention + LayerNorm + MLP with residuals) → $Y$.
- Note: **recent architectures use Rotary Positional Embedding (RoPE)** instead.

## 13.7 Transformers for images — Vision Transformer (ViT)

[Figure from the ViT paper by Dosovitskiy et al. — "An Image is Worth 16×16 Words"; the slide's caption cites it via "Attention is All You Need paper by Dosovitsky et al."]

Procedure for an $H \times W \times C$ image:

1. Split into a grid of patches $x_1, \dots, x_N$ (3×3 grid illustrated), each patch $x_i \in \mathbb{R}^{P\times P \times C}$; number of tokens $N = HW/P^2$.
2. **Linear patch embedding**: $z_i = \mathrm{flatten}(x_i)\, E$ with $E \in \mathbb{R}^{(P^2 C)\times D}$, giving $z_i \in \mathbb{R}^{1\times D}$.
3. Prepend a learnable **class token** $z_0$; add positional embeddings: $z_i^0 = z_i + E_{pos}^i$, $E_{pos}^i \in \mathbb{R}^{1\times D}$.
4. Run the transformer encoder: $X^L = \mathrm{TransformerEncoder}(X^0)$ with $X^0 = [z_0^0; z_1^0; \dots; z_9^0]$.
5. Classify from the final class token: $y = \mathrm{MLP}(z_0^L)$.

**CNNs vs. Transformers for images**:

- In CNNs, **locality and the two-dimensional neighborhood structure are strong inductive biases** for vision tasks.
- In ViT: **only the MLP layers are local, while self-attention is global**; **patching is the only 2-D inductive bias** — the model has to learn geometrical properties from scratch; consequently ViT **generally requires more training data** than a comparable CNN.

## 13.8 References

1. *Deep Learning: Foundations and Concepts*, Christopher Bishop & Hugh Bishop — Chapter 12 (Transformers), https://www.bishopbook.com/
2. *Understanding Deep Learning*, Simon J. Prince — https://udlbook.github.io/udlbook/

---
# 14. Lecture 11 — Pixel-Wise Predictions with Deep Neural Networks (`Lecture_11-Pixel_level_predictions.pdf`, 140 slides)

*Ertunc Erdil, ETH Zürich (slide date printed: May 6, 2025)*

**Five different pixel-level prediction problems**: segmentation · restoration · synthesis · registration (*not covered in this lecture*) · reconstruction (*not covered*). For each covered problem the same three-part strategy is followed: **(1) data set, (2) cost function, (3) architectures**.

## 14.1 Machine learning recap

- **Mapping**: features $x$ (observed at inference; real/categorical; $D$-dimensional — e.g., image intensities for classification, intensities at/around a pixel for segmentation) → labels $y$ (not observed at inference; real/categorical; $d$-dimensional — class of the image; label at a pixel). Parameterized approximation $y \approx f(x; \theta)$ (non-parametric mappings also possible; here: multi-layered neural networks).
- **Learning**: determine "best" parameters from **labeled examples** (supervised learning); training set $D_{tr} = \{(x_n, y_n)\}_{n=1}^N$ with ground-truth labels; generic loss $\mathcal{L}(y, f(x;\theta))$; training loss $\mathcal{L}(D_{tr};\theta) = \frac{1}{N}\sum_n \mathcal{L}(y_n, f(x_n;\theta))$.
- **Learning is optimization**: $\theta^* = \arg\min_\theta \mathcal{L}(D_{tr};\theta)$, which must also *generalize* to $D_{val}$. Note: the loss is a **sum over samples** — $\theta$ does not have to satisfy any single sample perfectly; it satisfies them **on average**.
- **Hyper-parameters and the validation set**: most algorithms have hyper-parameters $\phi$ — so really $y \approx f(x; \theta, \phi)$. For deep networks: architecture, stopping point of optimization, learning rate. The validation set determines them: $\phi^* = \arg\min_\phi \mathcal{L}(D_{val}; \theta^*, \phi)$ where $\theta^* = \arg\min_\theta \mathcal{L}(D_{tr}; \theta, \phi)$ — ideally solved as a **nested optimization**.
- **Prediction/inference**: for a new sample, $y \approx f(x; \theta^*)$; remember that in general $y \ne f(x;\theta^*)$. Prediction error is computed on a **hold-out test set**, either with the training loss $\frac{1}{T}\sum_t \mathcal{L}(y_t, f(x_t;\theta^*))$ or with another, more application-appropriate function $\frac{1}{T}\sum_t D(y_t, f(x_t;\theta^*))$ — used when $D$ is more appropriate but not differentiable (e.g., ranking-based).
- **CNN recap**: classification network $I \to L_1 \to \dots \to L_5 \Rightarrow O \in [0,1]$ (→ = convolution + non-linearity + pooling; ⇒ = fully connected). **Receptive fields**: the receptive field depends on the layer a neuron sits in; deeper neurons see larger input patterns — **large contextual information but perhaps fewer local details**.
- **Transformer recap**: $Y = \mathrm{Softmax}(QK^T/\sqrt{D_K})\, V$.

## 14.2 Segmentation

**Problem recap**: assign to each pixel a label indicating the structure it belongs to; **multi-modal** versions exist (multiple input sequences).

### Data sets

- Need labeled image/segmentation pairs — "and you need a *large* number"; also a validation set and a test set.
- **How big a data set do you need?** Generally no good answer — "it depends on the problem": how variable is the structure? the background? the intensity profile? **Example**: for brain MRI with T1w MPRAGE, it takes **3–5 labeled volumes** to train a good UNet for segmenting large anatomical structures (white matter, gray matter, hippocampus, …). Difficult to generalize from such examples.
- **Variations and challenges in reality**:
  1. **Ambiguity in labeling** — ambiguity/uncertainty in segmentation labels is very common (inter-rater disagreement).
  2. **Missing data in multimodal cases** — very common when problems require multiple sequences; you may need to **impute** missing data during training or testing — and how do you do that when the missing data are images?
  3. **Images from different "domains"** — centers, scanners, sequences: even images all acquired with similar sequences (T1w MPRAGE) can differ a lot in tissue contrast due to small scanner/protocol (sequence parameter) differences; changes can happen during training **and** during inference. (Addressed later in the course — domain adaptation.)

### Cost functions

- **Pixel-wise binary cross-entropy** — extension of binary classification loss; per sample:

$$\mathrm{BCE}(y, f(x;\theta)) = -\mathbb{1}(y{=}1)\log f(x;\theta) - \mathbb{1}(y{=}0)\log(1 - f(x;\theta))$$

with $f(x;\theta)$ the predicted probability of class 1. Pixel-wise extension — sum over pixels $r$: $\mathcal{L}(y, f(x;\theta)) = \sum_{r=1}^D \mathrm{BCE}(y(r), f(x;\theta)(r))$ — note the network output is itself **an image**.

- **Multi-class cross-entropy**: $\mathrm{CE}(y, f(x;\theta)) = -\sum_k p(y{=}k) \log f_k(x;\theta)$ — the network assigns a probability per class; if the ground truth has no uncertainty (most applications), $p(y{=}k)$ is 1 for exactly one class. Pixel-wise: $\mathcal{L} = \sum_r \mathrm{CE}(y(r), f(x;\theta)(r))$; dataset loss sums over samples.
- **The challenge with (binary) cross-entropy**: with **small structures** (relative to background or other classes), the loss decomposes as $\mathcal{L} = \mathcal{L}_{\Omega_{small}} + \mathcal{L}_{\overline{\Omega_{small}}}$ and very likely $\mathcal{L}_{\Omega_{small}} \ll \mathcal{L}_{\overline{\Omega_{small}}}$ since $|\Omega_{small}| \ll |\overline{\Omega_{small}}|$ — small structures are **dominated** by large ones.
- **Sørensen–Dice coefficient**: $\mathrm{DSC} = \frac{2|A \cap B|}{|A| + |B|}$ — used for evaluating segmentation quality; 1 at perfect overlap, 0 at none; **but not differentiable** as a set measure.
- **DSC (soft Dice) loss** [Milletari et al., *V-Net: Fully Convolutional Neural Networks for Volumetric Medical Image Segmentation*, 3DV 2016] — binary:

$$\mathcal{L}(y, f(x;\theta)) = \frac{2\sum_{r} y(r)\, f(x;\theta)(r)}{\sum_r y(r)^2 + \sum_r f(x;\theta)(r)^2}$$

multi-class: sum the same ratio per class $k$ with $p(y(r){=}k)$ and $f_k$. **Each class's DSC loss is equal in magnitude despite different sizes** — contrary to cross-entropy (solves the small-structure problem).

### Architectures

- Setup: image $D_1 \times D_2$; binary segmentation → input $D_1\times D_2$, output $D_1 \times D_2 \times k$ giving per-pixel class probabilities.
- **From classifier to segmenter**: start from the classification CNN $I \to L_1 \dots L_5 \Rightarrow O$. We want an image-sized output, so: **upsample within the network** ($\uparrow s$) converting an intermediate layer $L_5$ to the required image size (upsampling factor $s$ depends on the channel size at $L_5$). Nothing special about the last layer — upsampling can be placed at different depths; use "same" convolutions (padding, no pooling) to preserve dimensions. Example channel assignment: $I \to 32 \to 128 \to 256 \to 128 \uparrow s \to 32 \to O$, with 1-padded 3×3 convolutions; ReLU activations internally; final layer **sigmoid** so $O(r) \in [0,1]\ \forall r$.
- **Multi-class**: the output has multiple channels, each an image of per-class probabilities; probabilities must add up → **soft-max activation across channels at each pixel**: $f_k(x;\theta)(r) = \exp\{a_k(r)\}/\sum_j \exp\{a_j(r)\}$; the pre-softmax convolutions output as many channels as classes.
- **Challenge with this simple idea**: at $L_5$ so many pooling operations have happened that **details are lost** — the output loses resolution. Key question: **how to use contextual information while retaining high-resolution information?**
- **FCN with hierarchical links** (fully convolutional networks): combine outputs from **different scales** ($O_1 \uparrow 32$, $O_2 \uparrow 16$, $O_3 \uparrow 8$) to retain high-resolution details — improved details through aggregating information from different layers.
- **UNet**: encoder–decoder with down-/up-sampling by 2 at each level ($I \to L_1 \downarrow L_2 \downarrow L_3 \downarrow L_4 \to L_5 \uparrow L_6 \uparrow L_7 \uparrow L_8 \to O$) and **skip connections** between corresponding levels:
  - Skip connections **retain high-resolution information**; they consist of convolutions and the results are **concatenated** at the target.
  - The number of levels and layer compositions are arbitrary — they can change.
  - Layer legend: $\to\downarrow2$ = same-convolution + non-linearity + pooling; $\to\uparrow2$ = bilinear upsampling + same-convolution + non-linearity **or** transposed convolution + non-linearity; $\to$ = same-convolution + non-linearity; final $\to$ = same-convolution + sigmoid or soft-max.
- **Further alternatives**: UNet has been hugely successful; alternatives exist (e.g., **DeepMedic**); 3D extensions available; **transformer technologies within UNet structures are immensely successful** (integrating transformer blocks in the encoding path); architecture design remains an active research area.

## 14.3 Restoration

**Problem recap** [Yang et al., *IEEE TMI* 2018 — low-dose CT denoising]: removing noise from an image, image super-resolution, bias-field removal, etc. all fall into this category.

### Data sets

- Pairs of degraded/clean images, in "large" numbers + validation and test sets. Data-set size: again no good answer — depends on how variable the underlying data is, how difficult the denoising problem is, how variable the intensity profile is.
- Challenge list extended with **(4) ground truth may not be available**: e.g., the low-dose CT images in the example are **simulated**, not acquired — acquiring real ground truth may be impossible due to physical limitations of the acquisition system or because additional acquisitions would be **unethical** (extra radiation dose).

### Cost functions

- **Basic**:
  - Mean absolute error: $\mathrm{MAE} = \sum_r |y(r) - f(x;\theta)(r)|$
  - Mean squared error: $\mathrm{MSE} = \sum_r \|y(r) - f(x;\theta)(r)\|_2^2$
  - Normalized MSE: $\mathrm{NMSE} = \mathrm{MSE}/\sum_r \|y(r)\|_2^2$ or $\mathrm{MSE}/\|\max(y) - \min(y)\|_2^2$
  - Peak signal-to-noise ratio: $\mathrm{PSNR} = 10\log_{10}\frac{\max(y)^2}{\mathrm{MSE}}$
  - **SSIM** [Wang, Bovik, Sheikh, Simoncelli, *IEEE TIP* 2004]: $\mathrm{SSIM}(a,b) = \frac{(2\mu_a\mu_b + c_1)(2\sigma_{ab} + c_2)}{(\mu_a^2 + \mu_b^2 + c_1)(\sigma_a^2 + \sigma_b^2 + c_2)}$, computed over many patches $a, b$ from $y$ and $f(x;\theta)$; a combination of local **luminance, contrast, and structure** comparisons.
- **Limitation of basic losses**: MSE-type losses may not capture differences that make images look "**perceptually**" similar — an MSE-trained CNN produces over-smoothed output (Yang et al. TMI 2018 example: ground truth vs. noisy input vs. blurry MSE output).
- **Perceptual loss**: while it's unclear what "perceptual differences" means exactly, one can use neural networks to measure them: pass both $y$ and $f(x;\theta)$ through a **previously trained CNN** and compare intermediate features $\phi_j$: $\sum_j \mathrm{MSE}(\phi_j^y, \phi_j^f)$. The feature CNN can be a well-established network trained on natural images (e.g., **VGG**) or a network trained on CT images for another task; it can be much deeper. Distances between "**deep features**" measure differences in contextual information beyond pixel-wise differences. Effect shown: VGG-perceptual-loss output is much sharper than MSE output.
- **Adversarial loss**: another way to capture perceptual differences — a **distributional distance**, defined not between two samples but **over sets of samples**. A **discriminator** $D$ tries to identify whether an input image is real or produced by the "generator" network $f(\cdot;\theta)$; optimized as a min-max problem:

$$\min_\theta \max_\psi\ \mathbb{E}_{y\sim p(y)}[\log D(y;\psi)] + \mathbb{E}_{x\sim p(x)}[\log(1 - D(f(x;\theta);\psi))]$$

- **Combined effect** [Yang et al. TMI 2018]: training with **Wasserstein-GAN adversarial loss + VGG perceptual loss** gives the most realistic denoised low-dose CT results (vs. MSE).

### Architectures

- Most works use **fully convolutional networks (FCN) or UNet** structures [Yang et al. TMI 2018 architecture shown].
- **Residual architectures**: restorative information is added as a **residual** to the original or naively-restored image — e.g., residuals added to a linearly upsampled image to restore high-resolution details (well established across restoration problems).

## 14.4 Synthesis

**Problem recap** [Wolterink et al., MICCAI 2017 — MR-to-CT synthesis]: synthesizing a target image from a source image. Uses: **data imputation**; **reducing the need for irradiation** (CT/PET); **reducing the need for additional imaging**.

### Data sets

- Paired source/target images in "large" numbers + validation/test. Size: no general answer — depends on source variability and target variability. **Some synthesis problems are possibly not solvable** — e.g., generic MRI → PET synthesis.
- Challenge list extended further:
  1. Ambiguity in labeling — *a source image may not uniquely identify a target*.
  2. Missing data in multimodal cases.
  3. Different domains (centers, scanners, sequences).
  4. Ground truth not available for all questions.
  5. **Labels and features may not be paired!** — **unpaired data**: label images and input images may come from **different individuals**.

### Cost functions

Similar to restoration: MAE, MSE, NMSE, PSNR, SSIM, perceptual distance, adversarial loss. **Distributional losses (adversarial) are particularly useful for unpaired datasets.**

### Architectures

- **Paired case**: "good old UNet" is used quite often — same architecture legend as segmentation but the final layer has **no activation function** (regression output); works with any loss.
- **Unpaired case — CycleGAN-style**: two generators $f(x;\theta): x \to y$ and $g(y;\psi): y \to x$, plus two discriminators $D_x, D_y$. Total loss = adversarial terms + **cycle-consistency** loss:

$$\underbrace{\mathcal{L}_{GAN}(g, D_x, x, y) + \mathcal{L}_{GAN}(f, D_y, y, x)}_{\text{adversarial}} + \underbrace{\|x - g(f(x;\theta);\psi)\|_1 + \|y - f(g(y;\psi);\theta)\|_1}_{\text{cycle consistency}}$$

with $\mathcal{L}_{GAN}(f, D_y, y, x) = \min_\theta \max_\phi \mathbb{E}_{y\sim p(y)}[\log D_y(y;\phi)] + \mathbb{E}_{x\sim p(x)}[\log(1 - D_y(f(x;\theta);\phi))]$.

- Final slide: **paired vs. unpaired training** comparison.

---
# 15. Lecture 12 — Uncertainty Estimation in Deep Learning Models (`lecture12_uncertainty_estimation.pdf`, 153 slides / 47 numbered pages)

*Ender Konukoglu, ETH Zürich, May 12, 2026*

**Framing question**: predictions of machine learning models, like any other models, may be uncertain; quantifying this uncertainty for medical imaging applications can be crucial. How do we do this for deep learning models? Outline: (1) the problem; (2) mathematical treatment — posterior distribution, approximations, evaluation; (3) examples; (4) unintuitive behavior in high dimensions.

## 15.1 The problem — why uncertainty matters

- **Classification**: OCT eye-image classification [Kermany et al. 2018; Laves et al. 2019] — some cases are very difficult even for experts; disease effects can be very subtle; the image may not provide enough information for a clear classification → important to acknowledge this and **assign high uncertainty**.
- **Human–AI interaction** [Tschandl et al., *Nature Medicine* 2020 — skin cancer recognition]: deep learning benefits raters; classes predicted with **high confidence are taken more into account by raters**; *inaccurate but highly confident* predictions adversely affect raters — the study shows raters changed their opinions in favor of a wrong model prediction when the model was very confident. **It is crucial not to be highly confident when you are wrong.**
- **Segmentation**: images show clearly where objects are, but **boundaries are much less clearly defined** due to limited resolution and contrast; raters effectively use prior knowledge when delineating. Raters cannot agree on boundaries [Becker et al. 2019 — prostate segmented manually by 6 raters]. Why it matters [Carass et al. 2017 — MS lesions, 4 input sequences, 2 expert raters]: segmentation uncertainty → uncertainty in **lesion (disease) load quantification**; in longitudinal settings, global/local lesion-load differences drive assessment of **treatment efficacy** — uncertainty must be taken into account for correct assessment.
- **Detection**: brain-MRI metastases example — identifying *clearly normal* and *clearly abnormal* anatomy helps clinical workflow efficiency; some abnormalities are hard to discern from normal anatomy, require human readers, and should be flagged via **high uncertainty**.
- **Reconstruction from undersampled data**: an **ill-posed** problem — missing information must be complemented by priors, and priors can fill in the image **in multiple ways**; reconstruction uncertainty → uncertainty in morphological measurements; models **should not provide confident predictions for information missing from the data**.
- **Generally in model fitting**: uncertainty exists wherever data underdetermines the fit; **uncertainty in extrapolation is amplified further** by architecture and parameter choices.

## 15.2 Mathematical treatment

### Notation and sources of uncertainty

Notation: features $x$; labels $y$; network $f(x;\theta)$; parameters $\theta$; training set $D$; **model specification $M$** (architecture + hyper-parameters). Standard prediction $y \approx f(x;\theta^*)$ with $\theta^* = \arg\min_\theta \mathcal{L}(D;\theta)$. A **Bayesian approach** is taken (good resources by David MacKay, Christopher Bishop, Kevin Murphy).

**Sources of uncertainty in this setup**:

1. $x$ may not uniquely identify $y$.
2. $D$ may not uniquely identify a $\theta^*$.
3. $D$ is itself a random sample.
4. $M$ is probably not a unique model for the problem.
5. The combination of $D$, $f(\cdot;\theta^*)$ and $M$ may not solve the task perfectly — *(the lecturer notes this last one is strictly a source of per-sample error rather than uncertainty).*

**Approach with caution** (the lecturer's caveats): defining "uncertainty" is challenging — specific definitions in the literature are still debated; this treatment is one attempt, full of personal choices; but the topic is very important and should be studied despite the disagreements.

### The posterior distribution as the end-goal

The goal of uncertainty estimation is the characterization of the **posterior distribution** $p(y \mid x)$ — the distribution of all labels likely to be associated with $x$; from it one can evaluate $\mathbb{E}_{y|x}[y]$ or draw samples. Note $p(y|x)$ contains **no model or training-data components** — those must be marginalized. Full probabilistic model and factorization:

$$p(y, x, \theta, D, M) = p(M)\, p(D)\, p(\theta \mid D, M)\, p(x)\, p(y \mid x, \theta, M)$$

- $p(M)$: prior over model specifications; $p(D)$: probability of drawing the training set from the population; $p(\theta \mid D, M)$: parameter posterior — how well each $\theta$ explains $D$; $p(x)$: probability of observing $x$; $p(y \mid x, \theta, M)$: prediction distribution.
- Marginalization required:

$$p(y \mid x) = \int_M \int_D \int_\theta p(y \mid x, \theta, M)\, dp(\theta \mid D, M)\, dp(D)\, dp(M)$$

**All of these integrations are challenging**: $\int_M$ — a LOT of model choices, infeasible to evaluate the inner integrals for all; $\int_D$ — would require all possible training sets of size $|D|$, but we usually have only one; $\int_\theta$ — sounds feasible, but $p(\theta|D,M)$ is hard to get since $\theta$ is very high-dimensional.

### What standard training/inference does (in this language)

Fix one $M$ and one $D$ (no hyper-parameter optimization; the story is similar with it):

- Training: $\theta^* = \arg\min_\theta \mathcal{L}(D;\theta)$ ⇒ $p(\theta \mid D, M) \approx \delta(\theta - \theta^*)$ (Dirac).
- Inference: $y \approx f(x;\theta)$ ⇒ $p(y \mid x, \theta, M) \approx \delta(y - f(x;\theta))$.
- Then the marginalization collapses: $p(y \mid x, M, D) = \delta(y - f(x;\theta^*))$, so the "usual" prediction is $f(x;\theta^*) = \mathbb{E}_{y|x,M,D}[y]$ — a point mass with **no uncertainty at all**.

### Approximations

Two main directions: **(1) sampling instead of integration**, **(2) approximating posterior distributions.**

**Ancestral sampling** from $p(y, \theta, D, M \mid x) = p(M) p(D) p(\theta|D,M) p(y|x,\theta,M)$:

1. $D^s \sim p(D)$; 2. $M^s \sim p(M)$; 3. $\theta^s \sim p(\theta \mid D^s, M^s)$; 4. $y^s \sim p(y \mid x, \theta^s, M^s)$.

The $y^s$ are samples from the target distribution; point estimate $\hat y = \frac{1}{S}\sum_s y^s$. In reality even sampling from these distributions is challenging → combine with approximations of posteriors and priors.

**Approximating $p(D)$** — usually only one training set $D'$ is given. Options:

1. $p(D) \approx \delta(D - D')$.
2. Approximate sampling by drawing **smaller data sets from $D'$**: random subsets with or without replacement (bootstrap) [Efron & Tibshirani 1993], or **non-overlapping partitions** ($\cup D^s = D'$, $D^{s_1}\cap D^{s_2} = \emptyset$). This option is heavily used in **ensembling** methods, which show great prediction quality and uncertainty estimates.

**Approximating $p(M)$** — the model specification is chosen by us, so in theory we can sample it; priors over models have been investigated [Akaike 1998; van der Linde 2003]. Options:

1. $p(M) \approx \delta(M - M')$ at a preferred model.
2. Approximate sampling over models: select a number of preferred models (e.g., an FCN *and* a U-Net for segmentation), or **generate new models around a preferred one** (adding layers/channels to a U-Net, changing the encoder, removing skip connections). Covering all models is infeasible; the cost is the time/energy to train them all.

**Approximating $p(\theta \mid D, M)$** — via Bayes (with $M$ given):

$$p(\theta \mid D, M) = \frac{p(D \mid \theta, M)\, p(\theta \mid M)}{p(D)}$$

- $p(\theta|M)$: prior over parameters; $p(D|\theta,M)$: likelihood.
- Correspondences with training: usual training = **maximum likelihood** ($\arg\max_\theta \log p(D|\theta,M)$ ↔ minimizing $\mathcal{L}(D;\theta)$); with weight regularization (e.g., weight decay) it is **MAP** ($\arg\max_\theta \log p(D|\theta,M) + \log p(\theta|M)$, where $\log p(\theta|M)$ ↔ the regularizer $R(\theta)$). MAP = MLE when $p(\theta|M) = const$.
- **Difficulties specific to deep learning** (via $\max_\theta \log p(D|\theta,M) \Leftrightarrow \min_\theta \mathcal{L}(D;\theta)$):
  - $\mathcal{L}$ is highly non-convex with multiple local minima ⇒ $p(\theta|D,M)$ is possibly **multi-modal**.
  - Different initializations lead to very different "optimal" parameters ⇒ **modes can be far apart**.
  - $p(\theta|M)$ can remedy this a bit but rarely completely without sacrificing prediction performance.
  - Changing parameters in one layer changes what other layers should be ⇒ **strong statistical dependencies between elements of $\theta$** ⇒ factorized approximations $p(\theta|D,M) \approx \prod_i p(\theta_i|D,M)$ may be poor.
- **Three main options**:
  1. $p(\theta|D,M) \approx \delta(\theta - \theta^*)$ with one MAP estimate — assumes a unimodal, sharply peaked posterior (**most likely not true**).
  2. **Approximate sampling via multiple trainings**: $\theta^s = \arg\max_\theta \log p(D|\theta,M) + \log p(\theta|M)$ with a **different initialization per sample**, $\theta_0^s \sim p(\theta_0|M)$ (simple factorized prior) — assumes different initializations capture different posterior modes (**deep ensembles**).
  3. **Approximate the posterior around a MAP estimate with a known distribution**, $p(\theta|D,M) \approx p(\theta|\theta^*, M)$ — e.g., **Laplace approximation** $p(\theta|D,M) \approx \prod_i \mathcal{N}(\theta_i; \theta_i^*, \sigma)$ with $\sigma$ assigned after MAP estimation. Options 1–2 can be combined with 3.
- **Training with the distributional approximation (option 3, learned during training)**: without priors — $\arg\max_\mu \log p(D|\theta^l, M)$ with $\theta^l \sim \prod_i \mathcal{N}(\theta_i;\mu,\sigma)$ ($\sigma$ fixed to avoid trivial solutions); or with priors —

$$\arg\max_{\mu,\sigma}\ \log p(D \mid \theta^l, M) - D_{KL}\!\left[\textstyle\prod_i \mathcal{N}(\theta_i;\mu,\sigma)\ \Big\|\ \prod_i \mathcal{N}(\theta_i;\mu_0,\sigma_0)\right], \quad \theta^l \sim \mathcal{N}(\mu,\sigma)$$

— the objective corresponds to the **Evidence Lower Bound (ELBO / variational lower bound)** for $p(D|M)$ (variational Bayesian neural networks).

### Modeling $p(y \mid x, \theta, M)$ (rather than approximating it)

- Most neural networks are **deterministic** given parameters — no ambiguity in the output — so this term is something we *model*. The simple model $p(y|x,\theta,M) \triangleq \delta(y - f(x;\theta))$ suffices for deterministic prediction.
- But it can be improved to reflect that **multiple $y$'s can correspond to the same $x$** (super-resolution: many high-res images per low-res input; segmentation: multiple experts draw different maps). In the factorization this is the **only term modeling the dependency of $y$ on $x$** — the best place to integrate one-to-many relations.
- **Approach I — predict distribution parameters** (heteroscedastic modeling):

$$p(y \mid x, \theta, M) \triangleq \mathcal{N}(y;\ f(x;\theta),\ g(x;\theta))$$

with $f$ predicting the mean and $g$ the standard deviation (distribution chosen per application, e.g., Bernoulli for binary classification). Training maximizes $\log p(y|x,\theta,M)$; for the Gaussian case:

$$\max_\theta\ \left[-\log g(x;\theta) - \frac{1}{2}\frac{(y - f(x;\theta))^2}{g(x;\theta)^2}\right]$$

For image outputs: factorized per-pixel $p(y|x,\theta,M) \triangleq \prod_i \mathcal{N}(y_i; f_i(x;\theta), g_i(x;\theta))$. If statistical dependencies between output pixels must be modeled: **compound distributions with a lower-dimensional latent space** $p(y|x,\theta,M) \triangleq \int_z p(y|z,x)\, p(z|x)\, dz$, with $p(z|x)$ and $p(y|x,z)$ modeled by networks as in **variational autoencoders** [Kohl et al. 2018 — Probabilistic U-Net].

- **Approach II — generate samples instead of a distribution** [Wang et al. 2018]: the model takes a **random input** $z$: $y^l = f(x, z^l; \theta)$, $z^l \sim p(z)$ — no explicit distribution. To make the samples sensible, train with a **distributional (adversarial) loss**:

$$\min_\theta \max_\psi\ \sum_n \mathcal{L}(y_n, f(x_n;\theta)) + \lambda\left[\mathbb{E}_{y\sim p(y)}[\log D(y;\psi)] + \mathbb{E}_{y = f(x,z^l;\theta)}[\log(1 - D(y;\psi))]\right]$$

### Evaluation — possibly the most challenging component

How do we know the model's distribution truly captures uncertainty? We cannot access the true posterior. **Two main methods**:

1. **Comparison with observed samples** — when a data set has **multiple labels per image**: compute distributional distances between generated and observed samples, e.g., **Maximum Mean Discrepancy (MMD)** [Gretton et al. 2012] and **Generalized Energy Distance (GED)** [Kohl et al. 2018; Baumgartner et al. 2019]:

$$D^2_{GED} = 2\,\mathbb{E}[d(s, y)] - \mathbb{E}[d(s, s')] - \mathbb{E}[d(y, y')]$$

where $d(\cdot,\cdot)$ is DSC or IoU-based distance, $s, s'$ model samples, $y, y'$ real segmentations from the data set.

2. **Calibration error** — when multiple samples are not available (the most general case): measure whether the assigned uncertainty **reflects error** — correlation between uncertainty and test error. **Expected Calibration Error (ECE)**:

$$\mathrm{ECE}(D;\theta) \triangleq \mathbb{E}_r\big[\,|\mathbb{E}_{c|r}[c] - r|\,\big]$$

where $r$ is the confidence the model assigns and $c$ the indicator of prediction correctness (average gap between confidence and accuracy).

## 15.3 Examples from the literature

- **Uncertainty over $\theta$ and $y\mid x$** [Gal & Ghahramani 2016 — *Dropout as a Bayesian Approximation*; Tanno et al. 2017, 2020]: combine (1) **variational drop-out** to sample $\theta$ given $D, M$ (approximating $p(\theta|D,M)$) and (2) a **pixel-wise distribution** to model $p(y|x,\theta,M)$ — applied to neuroimage enhancement / dMRI super-resolution.
- **Segmentation** [Kohl et al. 2018 — *Probabilistic U-Net*; Baumgartner et al. 2019 — *PHiSeg*]: probabilistic **latent-space** models for $p(y|x,\theta,M)$, trained on data sets containing **multiple annotations per image**.
- **Sampling for inverse problems** [Adler & Öktem 2018 — *Deep Bayesian Inversion*]: a **sampler** modeling $p(y|x,\theta,M)$ that uses an adversarial loss to ensure outputs are realistic *and* satisfy the data.

## 15.4 Unintuitive behavior in high dimensions

*(Joint work with Tareen Dawood, Roberto Cagnotti, Kilian Zepf and Aasa Feragen — [Cagnotti 2023], [Zepf et al. 2026].)*

- Setting: **deep-decoder denoising** with noisy $x$ and noise-free estimate $\hat y = f(z|\theta^*)$, $\theta^* = \arg\max_\theta \|x - f(z|\theta)\|_2^2$ *(as printed; i.e., fitting an untrained network to the noisy image)*, $z \sim \mathcal{N}(0,\Sigma)$.
- **Sampling model parameters ("epistemic uncertainty")**: instead of a point estimate, formulate $p(\theta \mid x, z, M) \propto p(x \mid \theta, z, M)\, p(\theta|M)$ with $p(x|\theta,z,M) = \mathcal{N}(f(z|\theta), I)$ and sample [cf. Šidák 1967].
- **Typical sets**: for an isotropic unit Gaussian in $d$ dimensions, the expected distance of a sample from the mean is $\mathbb{E}[|x - \mu|] = \sqrt{d}$ [Blum, Hopcroft, Kannan — *Foundations of Data Science*]. As dimension increases, **samples move away from the mode** of the distribution.
- Consequence: posterior **samples move away from high-likelihood areas** — and since $p(D|\theta,M)$ *is* the task-specific loss being minimized, sampled parameters can perform poorly on the task. **Bounded sampling** [Zepf et al. 2026 — *Mixtures of Locally Bounded Langevin Dynamics for Bayesian Model Averaging*, TMLR 2026] may help but does not provide a complete solution.

## 15.5 References cited on the slides

Kermany et al. 2018 (Cell); Laves et al. 2019 (Current Directions in Biomedical Engineering); Tschandl et al. 2020 (Nature Medicine); Becker et al. 2019 (European Journal of Radiology); Carass et al. 2017 (NeuroImage); Efron & Tibshirani 1993 (*An Introduction to the Bootstrap*); Akaike 1998; van der Linde 2003; Kohl et al. 2018 (NeurIPS, Probabilistic U-Net); Wang et al. 2018 (CVPR, conditional GANs); Gretton et al. 2012 (JMLR, kernel two-sample test); Baumgartner et al. 2019 (MICCAI, PHiSeg); Tanno et al. 2017 (MICCAI), 2020 (NeuroImage); Gal & Ghahramani 2016 (ICML); Adler & Öktem 2018 (arXiv); Zepf, Dawood, Feragen, Konukoglu 2026 (TMLR); Cagnotti 2023 (ETH student paper); Šidák 1967 (JASA); Blum, Hopcroft, Kannan 2020 (*Foundations of Data Science*, CUP).

---
# 16. Lecture 13a — Domain Adaptation (`lecture13_domain_adaptation.pdf`, 101 slides / 42 numbered pages)

*Ender Konukoglu, ETH Zürich, May 19, 2026*

**Problem with generalization**: models trained with one training set do not perform well on another data set with different — even slightly different — intensity characteristics. Outline: the problem · domain adaptation approaches — naive approach, transfer learning, unsupervised domain adaptation, extreme data augmentation, unsupervised source-free domain adaptation.

## 16.1 The problem

- Focus on **segmentation** (pixel-wise class assignment from intensities): deep learning is clearly the state of the art across anatomies, pathologies, modalities; accuracies are **close to inter-rater variability** — e.g., pair-wise average DSC for whole-gland prostate segmentation in a study with 6 readers and 80 patient images was **0.74**.
- **When the domain changes**: even *slight* differences between training and test data statistics can lead to **substantial performance degradation** — neural networks are **not robust against "domain shifts."**
- Not specific to segmentation [images from McKinney et al., *Nature* 2020]: pertinent for restoration, synthesis, reconstruction, registration, …
- **Finer taxonomy of dataset shift** [table from Castro et al., "Causality Matters in Medical Imaging", *Nature Communications* 2020]:

| Type | Change | Examples of differences |
|---|---|---|
| Population shift | $P_D(Z)$ | ages, sexes, diets, habits, ethnicities, genetics |
| Annotation shift | $P_D(Y \mid X)$ | annotation policy, annotator experience |
| Prevalence shift | $P_D(Y)$ | case–control balance, target selection |
| Manifestation shift | $P_D(Z \mid Y)$ | anatomical manifestation of the target disease or trait |
| Acquisition shift | $P_D(X \mid Z)$ | scanner, resolution, contrast, modality, protocol |

- **Notation**: a **domain** = a distinct subset of samples showing similar variations (center, scanner, sequence, acquisition protocol). **Source domain** = subset used for training; **target domain** = subset on which the model is applied; both can consist of multiple subsets. Samples: $(x^{SD}, y^{SD})$, $(x^{TD}, y^{TD})$; datasets $D^{SD}$, $D^{TD}$.
- **Domain shift** definition: if target domain ⊂ source domain → algorithms should generalize; if target domain ⊄ source domain → **domain shift** — training happened on samples different from those seen at inference.

## 16.2 Naive solution — separate training per domain

- Source domain: train with many labeled samples $\{(x_n^{SD}, y_n^{SD})\}$, $\min_\theta \mathcal{L}(D^{SD};\theta)$; infer with $y^{SD} \approx f(x^{SD}; \theta^*_{SD})$. Target domain: same with its own many labeled samples → $\theta^*_{TD}$.
- **If enough labeled examples are present, this is the best-performing model.** But it requires a large labeled set **at every domain** where the algorithm is used — labeled samples are expensive: curation and labeling take a LOT of time.

## 16.3 Transfer learning (fine-tuning)

- Initial training on the source domain (many samples); **fine-tune** on the target domain with **very few** labeled samples:

$$\min_\theta \mathcal{L}(D^{TD}; \theta), \qquad \theta^{(0)} = \theta^*_{SD}$$

(training starts at the optimal source-domain parameters).

- Requires only a few labeled target-domain samples — but still requires labeled samples at **each** target domain.
- **Reducing the labeled-sample requirement further**:
  - *Alternative I*: use **unlabeled** samples (as in the semi-supervised learning setting).
  - *Alternative II*: **reduce the number of parameters trained during fine-tuning** (especially useful when unlabeled examples are unavailable):
    - Fix most parameters and only train the layers between input $I$ and $L_1$ (the input adapter).
    - Or change only the **batch-norm parameters**: $\mathrm{BN}(a_l) = \gamma \frac{a_l - \mu_l}{\sqrt{\sigma_l^2 + \epsilon}} + \beta$ — all other parameters stay the same.
  - Demonstration: **fine-tuning with only 4 images** achieves good target-domain segmentation.

## 16.4 Unsupervised domain adaptation (UDA)

- *Can we train models without any labels at the target domain?* Use **many unlabeled target samples** $\{(x_n^{TD}, \cdot)\}$ together with the labeled source set:

$$\min_\theta\ \mathcal{L}(D^{SD};\theta) + \mathcal{L}_{adv}(D^{TD}, D^{SD}; \theta)$$

- The **adversarial loss** ensures **similar features are extracted from source and target samples** — the network becomes *blind to differences between domains*. With a discriminator $D(\cdot;\psi)$ applied to intermediate features $l(x) = [L_2(x), L_3(x), L_4(x)]$:

$$\min_\theta \sum_n \mathcal{L}(y_n^{SD}, f(x_n^{SD};\theta)) + \max_\psi\ \mathbb{E}_{x\sim p_{SD}}[\log D(l(x);\psi)] - \mathbb{E}_{x\sim p_{TD}}[1 - \log D(l(x);\psi)]$$

- Interpretation: **supervised loss** = task-specific loss on labeled source samples; **adversarial loss** = distributional loss measuring distance between feature distributions of the two domains. Applicable to any architecture (only a simple one was shown).
- Properties: needs **no labeled target images**; but requires a **new training for each target domain**, and source-domain images must be transported to each target domain.

### Common aspects of the first three approaches

Naive, transfer learning, and UDA **all require training at the target domain**; training requires some (labeled) data. Transfer learning reduces the number of labeled target samples; UDA removes labeled target samples entirely — at the price of using **source samples in every target training**. The next two approaches do **no labeled training at the target domain at all**.

## 16.5 Domain generalization via (extreme) data augmentation

- **Domain generalization framework**: data augmentation and test-time adaptation methods **train only in the source domain**, require no target-domain samples for training, and at most **adapt** at the target domain.
- **Data augmentation** (well established): transformations of data samples serve as new samples — very effective against lack of data. Geometric transformations (affine, elastic); intensity transformations (blur, added noise, gamma correction $I^\gamma$).
- For **domain generalization**: train on $D^{SD} \cup \hat D^{SD}$ (original + augmented); at the target domain there is **no training at all** — inference uses $y^{TD} \approx f(x^{TD}; \theta^*_{SD})$ directly.
- **Hypothesis: augmentation can mimic new domains.** **Stacked transformations**:

$$(\hat x_s, \hat y_s) = \tau^n_{p_n, m_n}(\tau^{n-1}_{p_{n-1}, m_{n-1}}(\cdots \tau^1_{p_1, m_1}((x_s, y_s))))$$

each transformation $\tau^n$ applied with probability $p_n$ and magnitude $m_n$. Categories:

  - *Image quality*: sharpness, blurriness, noise level
  - *Image appearance*: brightness (intensity shifts), contrast (gamma correction)
  - *Spatial configuration*: rotation, scaling, deformations
  - The effect of each transformation on the **labels** is well known: spatial transformations transform labels too; appearance changes do not affect labels.
- Demonstrated on prostate segmentation in MRI and left-ventricle segmentation in ultrasound.
- **Pictorial view**: augmentation expands the source image set $X_{SD}$ into $\tilde X_{SD}$, *hoping* the expanded set overlaps with the target set $X_{TD}$. A very strong method — but performance improvement **may be limited to the variations covered by the augmentation strategy**.

## 16.6 Unsupervised source-free domain adaptation — test-time adaptation (TTA)

- Source domain: train (with augmentation) as before. Target domain: **no training samples at all**; at inference, adapt a subset of parameters $\phi$ by minimizing a label-free cost on the test sample(s), then predict:

$$\min_\phi\ \mathcal{L}\big(f(x^{TD}; \theta^*_{SD}, \phi)\big) \quad\text{then}\quad y^{TD} \approx f(x^{TD}; \theta^*_{SD}, \phi^*)$$

**Two main questions**: (1) *what cost function to optimize?* (2) *which parameters $\phi$ to train/fine-tune?*

### Question 1 — the cost function (three families)

1. **Entropy of the prediction** [Wang et al., ICLR 2021 — "Tent"] (classification): $H(f(x;\theta)) = -\sum_k f_k(x;\theta)\log f_k(x;\theta)$.
   - Assumption: pushing prediction confidence higher leads to good domain generalization.
   - Pros: simple, no prior information, applicable to different tasks. Cons: a **strong assumption** — holds only if the prediction is not far off and if you adapt to **many test samples together**.
2. **Auto-encoder loss on input, output, and intermediate features** [He et al., MICCAI 2020]:
   - An auto-encoder maps an image to itself, $x \approx D(E(x))$; it reconstructs well **only for samples from the domain it was trained on**.
   - AEs ($AE_I, AE_1, AE_2, AE_3, AE_O$ attached to $I, L_1\dots, O$) are trained on the **source** domain; at test time the network parameters are adapted so that inputs/features/outputs are reconstructed accurately → they have been converted to *source-domain-like* representations → the network predicts accurately.
   - Pros: generally applicable, a strong prior model. Cons: input and intermediate features **can be prone to domain shifts themselves**.
3. **Segmentation prior evaluated at the output** (plausibility of the result) [Karani et al., *Medical Image Analysis* 2021]:
   - A **denoising auto-encoder (DAE)** maps a noisy version of an image to itself, $x \approx D(E(\tilde x))$; trained **on segmentation maps** it learns a **prior over plausible segmentations**.
   - Adaptation objective: change network parameters so the network's output is accurately reconstructed by the source-trained DAE — $\mathcal{L}\big(f(x;\theta,\phi),\ H_{\psi^*_{SD}}(f(x;\theta,\phi))\big)$ — converting outputs to source-like (plausible) segmentations.
   - Pros: a strong prior gives accurate results; **segmentation maps are unaffected by intensity changes**, so the prior itself is immune to this kind of domain shift. Cons: **specific to the segmentation task**.

### Question 2 — which parameters to adapt

- **All parameters** ($\phi = \theta$): maximally flexible — but too much flexibility can produce outputs that perfectly satisfy the cost while **no longer being related to the input**.
- **A subset** ($\phi \subset \theta$): less flexible, avoiding that issue; also desirable because the initial parameters were obtained from large curated data sets — costly in time, data, and money — and shouldn't be changed if not needed.
- **Batch-norm parameters only**: very simple, very few parameters; but batch-norm parameters in **deeper layers** can affect results a lot, and it is unclear whether they address intensity-characteristic changes.
- **A shallow "adaptation" module prepended to the network**: $f(x; \theta, \phi) = f(N(x;\phi); \theta)$ — the shallow module is not too flexible and pre-trained parameters stay untouched; cost: the network becomes slightly bigger.
- Final slides: **a complete TTA model** (combining the pieces) and **TTA in action** on example images.

---
# 17. Lecture 13b — Learning from Unlabeled Examples (`lecture13_semi_supervised.pdf`, 101 slides / 44 numbered pages)

*Ender Konukoglu, ETH Zürich, May 19, 2026*

**Framing**: a large number of labeled examples is crucial for deep learning, but in medical applications large data sets are not common — *how can we learn accurate deep learning models from unlabeled examples?* Outline: the problem · noise-to-noise · self-training · teacher–student models · auxiliary losses · self-supervised learning.

## 17.1 The problem

- Supervised deep learning owes its success to **large labeled data sets** — the best-known demonstrations use over **1M labeled examples**; this applies to images as well as text — the current success of language models comes from using "(seemingly) the entire internet!"
- **Generating very-large-scale labeled data sets is not feasible for many medical applications**, for several reasons (built up over successive slides):
  - Manual annotations are **very costly** — segmentations are mostly done manually by experts (people who know what they are segmenting).
  - The nature of the problem may not permit large labeled sets — **ground-truth images may be very costly to acquire** (e.g., in the low-dose CT example, image pairs may be obtainable from some people but not all).
  - The ground truth may be **impossible to acquire at all** — more often in biological applications, the noisy images are the only ones obtainable, yet noise must be removed.
  - Common thread: **labeling is the problem — expensive, not feasible, or impossible.** Models that learn from few labeled examples would be very valuable.
- **Problem setup**: *few labeled examples* (getting a few labeled images is often possible; unpaired data sets can help for some problems, e.g., CT synthesis from MRI; more accuracy with fewer labels is always better) + *many unlabeled examples* (images without labels — e.g., images without ground-truth segmentations; often available in large numbers, though for some problems even unlabeled data is scarce).

## 17.2 Noise2Noise — learning to denoise without clean targets

- Can a restoration model be learned **only from corrupted samples**? Usual training uses clean images: $\arg\min_\theta \sum_n \mathcal{L}(y_n, f(x_n;\theta))$.
- Suppose instead of a clean image we have a **distribution of noisy images** $p(\hat y)$ for the same underlying sample. Minimizing $\arg\min_{f(x;\theta)} \mathbb{E}_{\hat y}[\|f(x;\theta) - \hat y\|^2]$ yields

$$f(x;\theta) = \mathbb{E}_{\hat y}[\hat y]$$

(the L2-optimal prediction is the *mean* of the noisy targets).

- Extending over samples $x$: $\arg\min_\theta \mathbb{E}_x\{\mathbb{E}_{\hat y | x}[\|\hat y - f(x;\theta)\|^2]\}$ — this minimizes the distance between the network output and *noisy* samples of $p(\hat y \mid x)$, never using a clean image. **If $\mathbb{E}[\hat y \mid x] = y$** (the average of noisy samples is the clean sample), then, given infinite data, this objective and the supervised objective $\arg\min_\theta \mathbb{E}_{x,y}[\|y - f(x;\theta)\|^2]$ **give the same solution**.
- **Practical cost function**: replace $\arg\min_\theta \sum_n \|y_n - f(x_n;\theta)\|^2$ with

$$\arg\min_\theta \sum_n \sum_m \|y_n^m - f(x_n;\theta)\|^2$$

where $y_n^m$ are noisy versions of the ideal $y_n$ (train noisy→noisy).

- Application: **Noise2Noise for restoration of CT and MRI**; further variations and improvements exist (one proposed in the referenced article).

## 17.3 Self-training (pseudo-labeling)

A simple way to use unlabeled images — iterative algorithm:

1. Train the network with labeled examples: $\theta_0^* = \arg\min_\theta \sum_n \mathcal{L}(y_n, f(x_n;\theta))$.
2. Predict **pseudo-labels** for unlabeled images at iteration $i$: $\hat y_m = f(x_m; \theta_i^*)$.
3. Retrain with labeled + pseudo-labeled data: $\theta_{i+1}^* = \arg\min_\theta \sum_n \mathcal{L}(y_n, f(x_n;\theta)) + \lambda \sum_m \mathcal{L}(\hat y_m, f(x_m;\theta))$ ($\lambda$ a weighting factor).
4. Iterate steps 2–3.

- **Why should it work? Entropy minimization**: MNIST digit classification t-SNE plots of intermediate representations show that self-training leads to **compact class representations with low-density regions between classes**.
- **Caveats**: works well **if the initial model's estimates are not bad** (entropy minimization assumes mostly correct class assignments); if the initial estimates are poor, self-training **quickly diverges** [Chapelle, Schölkopf, Zien, *Semi-Supervised Learning*, 2009]. Additional regularization of the pseudo-labels can prevent divergence — but even then the model can diverge quite often.
- **Self-training for cardiac MRI segmentation** — one modification: **post-process pseudo-labels** before retraining: $\hat y_m = f(x_m;\theta_i^*)$, $\tilde y = g(\hat y_m)$, then retrain on $\tilde y_m$. In the referenced work $g(\cdot)$ was a **Conditional Random Field (CRF)** optimization — "but even a median filter would work."

## 17.4 Teacher–student models

- A new type of regularization: **two interacting networks** generate pseudo-labels, one evolving more slowly than the other [Tarvainen & Valpola, NeurIPS 2017].
- Components: **student** $f_s(x;\theta)$; **teacher** $f_t(x;\phi)$; supervised cost on labeled examples $\mathcal{L}(y, f_s(x;\theta))$; **consistency loss** $\mathcal{L}_c(f_s(x;\theta), f_t(x;\phi))$ — applicable to **both labeled and unlabeled** examples. Total student loss:

$$\sum_n \mathcal{L}(y_n, f_s(x_n;\theta)) + \lambda\left(\sum_n \mathcal{L}_c(f_s(x_n;\theta), f_t(x_n;\phi)) + \sum_m \mathcal{L}_c(f_s(x_m;\theta), f_t(x_m;\phi))\right)$$

($n$ = labeled, $m$ = unlabeled samples).

- **Mean teacher**: teacher and student are trained alternately; many teacher-update rules are possible (e.g., $\phi^i = \theta^i$); in the best-known **mean-teacher** model the teacher is the **exponential moving average** of the student:
  1. For the given teacher, train the student (minimize the total loss above with $\phi^{i-1}$ fixed).
  2. Update the teacher: $\phi^i = \alpha \phi^{i-1} + (1-\alpha)\, \theta^i$.
  3. Iterate.
- Results: fairly good segmentation models in medical imaging — e.g., brain-lesion segmentation with **only 20 labeled and 196 unlabeled samples**.

## 17.5 Auxiliary losses

The teacher–student total loss can be viewed as **supervised loss + auxiliary loss**. This form generalizes: construct auxiliary losses believed useful for the task. Two classes:

- **Semi-supervised type** — defined over unlabeled images *during task-specific optimization*.
- **Self-supervised type** — defined over unlabeled images *independent of the task*.

### Consistency under transformations (semi-supervised auxiliary loss)

- In pixel-wise prediction, spatial transformations of the input should be matched at the output: if $\phi$ is a spatial transformation (affine or non-linear deformation), $x \Leftrightarrow y \Rightarrow \phi \circ x \Leftrightarrow \phi \circ y$ — the principle underlying data augmentation.
- For each image sample two transformations $\phi_1, \phi_2$ are drawn, giving $\phi_1 \circ x$, $\phi_2 \circ x$, $f(\phi_1 \circ x;\theta)$, $f(\phi_2 \circ x;\theta)$; **consistency loss** (requires no labels):

$$\mathcal{L}_{cons}(x, \phi_1, \phi_2; \theta) = \mathcal{L}\big(\phi_2 \circ \phi_1^{-1} \circ f(\phi_1 \circ x;\theta),\ f(\phi_2 \circ x;\theta)\big)$$

- Final semi-supervised cost (second sum over labeled **and** unlabeled examples):

$$\sum_n \mathcal{L}(y_n, f(x_n;\theta)) + \sum_m \mathbb{E}_{\phi_1,\phi_2}\left[\mathcal{L}\big(\phi_2 \circ \phi_1^{-1} \circ f(\phi_1 \circ x_m;\theta),\ f(\phi_2 \circ x_m;\theta)\big)\right]$$

- Architecture diagram + results: **chest radiograph segmentation** with 100 total samples, using different labeled portions (5, 10, …, 100) with the rest unlabeled.

## 17.6 Self-supervised learning

- **Main principle**: train a network on a **pre-text task that requires only images**; the trained network is then suitable for **fine-tuning with very few labeled examples** on other tasks. The crucial problem: identify a pre-text task that yields useful parameters. [Example images from Gidaris et al., ICLR 2018 (rotation prediction) and Pathak et al., CVPR 2016 (context encoders/in-painting).] Underlying assumption: such tasks are good for learning "generalizable" parameters.
- Example: **"Rubik's cube" pre-text task for medical image analysis** (solving shuffled 3D sub-cubes).
- These tasks are **hand-crafted** — assumed good, empirically shown to work to some extent; a **principled task** would be preferable.

### Contrastive learning

- Borrow the idea of forcing compact representations with **surrogate labels** [Hadsell et al., CVPR 2006; Chen et al., ICML 2020 — SimCLR]: **random transformations of the same image are "close"** and should be close in representation space.
- **Contrastive (InfoNCE-style) loss**:

$$\mathcal{L}(x, \{\hat x\}, \phi_1, \phi_2) = -\log \frac{e^{\rho(h(\phi_1 \circ x),\, h(\phi_2 \circ x))/\tau}}{e^{\rho(h(\phi_1 \circ x),\, h(\phi_2 \circ x))/\tau} + \sum_n e^{\rho(h(\phi_1 \circ x),\, h(\hat x_n))/\tau}}$$

  - Similarity via **cosine similarity** $\rho(a,b) = a^T b / (\|a\|\|b\|)$; temperature $\tau$.
  - Transformations $\phi$ are random and **not restricted to geometric** ones — they include intensity changes.
  - $\{\hat x\}$ = the **"negative set"** — samples that should *not* share a representation with $x$.
  - Minimizing the loss: the numerator **maximizes** similarity between views of the same content ($\phi_1 \circ x$, $\phi_2 \circ x$); the denominator **minimizes** similarity to different-content samples.
- **Global contrastive learning**: train an encoder ($x \to L_1 \dots L_5 \to h(x)$) with this loss — image-level representations at $L_5$ become compact w.r.t. the surrogate labels/closeness notion.
- **Local contrastive loss**: additionally force **local** representations to be compact — particularly useful for pixel-wise predictions. Train a **decoder** attached to the fixed encoder, applying the contrastive loss per patch $P_i[\cdot]$ (the $i$-th patch of the feature/image):

$$\mathcal{L} = -\sum_i \log \frac{e^{\rho(P_i[h(\phi_1\circ x)],\, P_i[h(\phi_2 \circ x)])/\tau}}{e^{\rho(P_i[h(\phi_1\circ x)],\, P_i[h(\phi_2\circ x)])/\tau} + \sum_n e^{\rho(P_i[h(\phi_1\circ x)],\, P_i[h(\hat x_n)])/\tau}}$$

  - **Attention with transformations**: they must not change patch correspondences — either restrict to correspondence-preserving transformations, or **undo them** when computing correspondences: compare $P_i[\phi_1^{-1} \circ h(\phi_1 \circ x)]$ with $P_i[\phi_2^{-1} \circ h(\phi_2 \circ x)]$.
- **In action for MRI segmentation**: results from [Chaitanya et al., NeurIPS 2020] — global + local contrastive pre-training improves segmentation with few labels.

### Masked autoencoders (MAE)

- Akin to **in-painting**: reconstruct occluded parts of images — the model learns to predict missing information.
- Structure: a **larger encoder** processes the visible parts; a **smaller decoder** reconstructs the occluded parts. Parts of the image are **masked randomly** — the randomness encourages robust, generalizable representations. The trained encoder serves as an effective **backbone for downstream tasks**.
- **MAE in action** (courtesy of [Buess et al., MIDL 2024]): preliminary results show MAEs can be effective in learning generalizable representations; experiments continue.

### Foundation models

- **Self-supervised training at a very large scale**; **transformer-based architectures**; trained with **masked auto-encoding** (similar to in-painting) and **variations of contrastive learning**.

**Final message**: *"This is an active area of research."*

---
# 18. Lecture 14 — XAI in Medical Image Analysis (`Lecture_Interpretability_Reyes_ETHZ_2026.pdf`, 88 slides)

*Mauricio Reyes, PhD*

## 18.1 Definitions and motivation

- Opening context: DARPA's **Explainable AI (XAI)** program framing (today's black-box learners vs. XAI's "explainable model + explanation interface").
- **What is interpretability?** An interpretable ML algorithm is one where **the link between the features used by the model and the prediction itself can be understood by a human**.
- Another definition [Barocas, Friedler, Hardt, Kroll, Venkatasubramanian, Wallach — FAT-ML workshop series 2018]: **produce explanations without sacrificing accuracy** — simpler is easier to understand, but oversimplified models are typically uninteresting from an accuracy point of view.
- **Principle of "least effort" / shortcut learning**: what a model learns is shaped by its **inductive bias** — model architecture, loss, optimization, training data. Example shown: chest X-ray classifier attending to **laterality markers** — attention outside the region of interest.
- **Explaining DL decisions via attention maps** [Ribeiro et al. 2017; Zech et al. 2018]: a pneumonia classifier's saliency revealed the network **learned to detect the clinical site** (hospital-specific tokens/markers) rather than pathology.
- AI can predict **race from medical images** with high performance across multiple modalities; detection is not due to proxies or imaging-related covariates; the pattern persists across all anatomical regions [referencing the "AI recognition of patient race" work] — a strong reason to audit what models learn.

## 18.2 Case study — bias detection with interpretability

Automated classification of **low- vs. high-grade gliomas (LGG vs. HGG)** from T1c and T2 MRI [Pereira et al., MICCAI-iMIMIC 2018; Reyes et al., *Radiology: Artificial Intelligence* 2021]:

- Q: *Are there biases stemming from the data-preparation process?* A: **Yes — bias of learned patterns detected via interpretability** (saliency maps before/after correction shown).
- Key experiment: with intensity **normalization computed over the brain area only**, AUC = **0.8857**; with normalization over the **whole image**, AUC = **0.9841** — suspiciously better, and saliency shows the model attending outside the brain → the model likely entered **"shortcut learning" mode** (exploiting preprocessing artifacts).
- **Key finding: interpretability enhances data preparation *and* AI performance.**

## 18.3 Taxonomy of XAI methods

- **By access to the model**:
  - **Black-box operating methods** — no access to model internals needed; also called **model-agnostic**.
  - **White-box methods** — require access to internals; **gradient-based** methods are one example.
- **By output**:
  - **Visualization / saliency maps** — pixel-wise values reflecting importance to the model's prediction.
  - **Concepts** — a summary statement or keyword.
  - **Feature importance** — expressing the importance of features (e.g., high model weights).
- (Recap slides: typical CNN structure — convolution, pooling/subsampling, fully connected layers.)

## 18.4 Method gallery

1. **LIME — Local Interpretable Model-agnostic Explanations** [Ribeiro et al., "Why Should I Trust You?", 2016/2017]: explain any classifier $f$ locally by fitting a **potentially interpretable model** $g$ (e.g., sparse linear model over superpixels) that approximates $f$ in a neighborhood of the input, weighted by a **proximity measure**, plus a **complexity measure** penalty on $g$ (objective: fidelity $g \approx f$ + simplicity).
2. **Class-prototype maximization** [Simonyan et al. 2013 — *Deep Inside Convolutional Networks*]: a **global** explanation technique — find the input pattern maximizing a class activation (gradient ascent on the input); the result is **not necessarily a real image**. Also introduced gradient-based **saliency maps**.
3. **Synthesizing preferred inputs via deep generator networks** [Nguyen et al. 2016]: regularize activation maximization with a **generative prior** — pipeline: forward pass, detect maximum activation, backward pass into the feature space (e.g., FC6) of a generator; prior generator and DNN can be trained separately; yields much more natural preferred inputs. [Slide source: Wojciech Samek.]
4. **Grad-CAM** [Selvaraju et al. 2017]: **visual explanations via gradient-based localization** — a **generalization of CAM** [Zhou et al., CVPR 2016] applicable to **any CNN** (CAM requires the conv feature maps → global average pooling → softmax structure). Motivation: good visualization is **class-specific and detailed**. Computation: a single forward and (partial) backward pass; **neuron importance weights** $\alpha_k^c$ = global-average-pooled gradients $\partial y^c/\partial A^k$ of the class score $y^c$ (before softmax) w.r.t. feature map $A^k$; heatmap = ReLU of the weighted sum of feature maps. Also shown: robustness analysis on **adversarial examples** where VGG was "fooled."
5. **Meaningful perturbation** [Fong & Vedaldi 2017/2018 — *Interpretable Explanations of Black Boxes by Meaningful Perturbation*]: motivation — gradient-based saliency is not specific enough. Key idea: find the **right perturbation** (a type of **information deletion** — blur/noise/masking) and study its effect on $f(x)$: optimize an **"information-deletion" mask** that minimally covers the image while maximally dropping the class score (with a term enforcing minimal mask size). The meaning of the explanation depends on the meaning of the changes applied to the input; caveat: can lead to **unnatural perturbations** → adversarial-like artifacts against explanations.
6. **TCAV — Testing with Concept Activation Vectors** [Kim et al. 2017]: concept-based, human-friendly explanations. Define a concept $C$ by example images; learn a **concept activation vector** (normal to a linear classifier separating concept activations from random ones in layer $l$); **conceptual sensitivity** of class $k$ to concept $C$ = directional derivative of the class-$k$ logit at layer $l$ along the CAV; the **TCAV score** = fraction of class-$k$ inputs whose layer-$l$ activation was **positively influenced** by concept $C$. TCAV **facilitates spotting dataset biases**. [Image source: Been Kim.]

## 18.5 Shortcut learning, alignment, and fairness

- *What is AI learning — and what if it differs from human reasoning/knowledge?* Depending on the case this is a **shortcut**, a **bias**, or **knowledge discovery**.
- **"Right for the Wrong Reasons: Shortcut Learning as a Barrier to Clinical Deployment"** [Baniecki et al. 2024]: illustrates **human–AI alignment vs. misalignment** of attention with clinically meaningful regions.
- **Reliable clinical AI: fair and trustworthy — "Primum non nocere" (first do no harm)**: example of a **fairness gap** — AUC differences between male and female sub-groups (AUC range ≈ 81–84%, fairness-gap axis ≈ 2.2–3.6%); goal: close the gap. Achieving this needs the right **inductive bias** — a **balance between guidance and pattern discovery** ("Alignment").

### XAI-based inductive bias — SIBNet and hybrids

- Chest X-ray multi-label classification (classes: **atelectasis, pleural effusion, edema, cardiomegaly, consolidation**) with **class-specific attention maps** [Luo et al., MICCAI 2025 shown alongside].
- **SIBNet — Saliency Inductive Bias** [Mahapatra et al., *Medical Image Analysis* 2022]: use saliency as an inductive bias during training. Setup: five-fold validation, five classes, DenseNet-121 backbone. AUC comparison (~0.84–0.96 axis): SIBNet **outperforms** the DenseNet-121 CE baseline, Pham et al. 2020 (top-1 CheXpert challenge entry), Woo et al. 2018 (Convolutional Block Attention — channel & spatial attention), and Hu et al. 2018 (Squeeze-and-Excitation — channel attention). Interpreted as **indirect human–AI alignment**.
- **Combining human–AI alignment and XAI-based inductive bias** [Shu et al., MICCAI-iMIMIC 2025]: alignment guided by a **VLM (vision-language model)**; F1 progression reported: baseline **67.8** → hybrid variants **68.2** → **69.4**, compared against GAIN [Li et al., CVPR 2018 — human-AI guidance] and KAD [Zhang et al., *Nature Communications* 2023 — human-based knowledge-graph guidance].

## 18.6 Saliency as a model fingerprint; XAI-boosted active learning

- **Saliency maps evolve during training** (snapshots at 10/20/40/60/90% of training, uncertainty + saliency): **key message — saliency maps can be seen/used as a fingerprint of the model's behavior to inputs.**
- **GESTALT — Graph Node-Based Interpretability Guided Sample Selection**: interpretability-guided **active learning**. Pipeline: pre-trained classifier → saliency-map generator over a pool of samples → build a **class-specific graph** of saliency maps → graph-aggregation variants (1) GESTALT-Node, (2) GESTALT-Edge-Weight, (3) GESTALT-Weighted-Node → rank samples → top-ranked selection → update classifier. Guiding intuition: **"a sample is informative if the model struggles with it"** (easy samples have clean, focused saliency; hard/informative ones show the model struggling — "I can learn from it!"), optionally combined with **GenAI**-generated samples.
- **XAI-boosted active learning** results [Shu et al., MICCAI-iMIMIC 2024; Mahapatra et al., *IEEE TMI* 2022; *MedIA* 2024; *MedIA* 2024b]: AUC vs. percentage of training samples (0–100%) — **GANDALF** (Graph-based transformer and Data Augmentation Active Learning) reaches near-fully-supervised AUC with a fraction of the labels, outperforming random selection and uncertainty-estimation-based selection.

## 18.7 Wrap-up

- Interpretability of AI systems is propelled by new findings from the DL community to **safely translate the technology to (medical) applications**.
- The goal of interpretability is to have **enough information for the task at hand** — in medicine, **patient safety is first**.
- There are several types of XAI methods (taxonomy above).
- **Current challenges**: XAI needs to be **multimodal and longitudinal** to match clinical workflows.
- **XAI in the era of foundation models: a bigger black box?**

## 18.8 Bibliography (as listed on the slides, condensed)

Adadi & Berrada 2018 (IEEE Access XAI survey); Adebayo et al. 2018 (*Sanity Checks for Saliency Maps*); Barocas et al. 2018 (FAT-ML); Caruana et al. 2015 (KDD, intelligible models for healthcare); Datta, Sen & Zick 2016 (IEEE S&P, quantitative input influence); Doshi-Velez & Kim 2017 (*Towards a Rigorous Science of Interpretable ML*); Eaton-Rosen et al. 2018 (biomarker uncertainty); Gale et al. 2018 (radiologist-quality reports); Gallego-Ortiz & Martel 2016 (rules from tree ensembles, breast MRI); Gillies, Kinahan & Hricak 2016 (Radiomics, *Radiology*); Gilpin et al. 2019 (*Explaining Explanations*); Goldstein et al. 2015 (ICE plots, JCGS); Goodman & Flaxman 2017 (EU "right to explanation", AI Magazine); Gunning 2017 (DARPA XAI); Guo et al. 2017 (ICML, calibration of modern NNs); He et al. 2019 (*Nature Medicine*, practical AI in medicine); Hosny et al. 2018 (*Nature Reviews Cancer*, AI in radiology); Jiang et al. 2018 (NeurIPS, to trust or not to trust a classifier); Jungo et al. 2018 (MIDL, uncertainty-driven sanity check for brain-tumor cavity segmentation); Kim et al. 2017 (TCAV); Kindermans et al. 2017 (PatternNet/PatternAttribution); Kleesiek et al. 2016 (*Scientific Reports*, virtual raters); Koh & Liang 2017 (influence functions); Lambin et al. 2017 (*Nature Reviews Clinical Oncology*, radiomics); Lapuschkin et al. 2019 (*Nature Communications*, "Clever Hans" predictors); Letham et al. 2015 (Bayesian rule lists, stroke prediction); Lipton 2016 (*The Mythos of Model Interpretability*); Litjens et al. 2017 (*MedIA*, survey of DL in medical image analysis); Mahapatra et al. 2018 (MICCAI, active learning with conditional GANs); Maier-Hein et al. 2016 (crowd-algorithm endoscopic annotation); Miller 2017 (explanation & social sciences); Moosavi-Dezfooli et al. 2016 (DeepFool, CVPR); Murdoch et al. 2019 (interpretable ML definitions/methods); Nair et al. 2018 (uncertainty for MS lesion detection); Nguyen, Yosinski & Clune 2015 (CVPR, DNNs easily fooled); Parikh, Obermeyer & Navathe 2019 (*Science*, regulation of predictive analytics); Pereira et al. 2018 (*MedIA*, interpretability of RBM–random-forest brain-lesion features); Poursabzi-Sangdeh et al. 2018 (manipulating/measuring interpretability); Ribeiro, Singh & Guestrin 2016 (LIME); Selvaraju et al. 2017 (Grad-CAM, ICCV); Simonyan, Vedaldi & Zisserman 2013 (saliency maps); Szegedy et al. 2013 (*Intriguing properties of neural networks*); Topol 2019 (*Nature Medicine*, high-performance medicine); Van Lent, Fisher & Mancuso 2004 (explainable AI for tactical behavior); Zech et al. 2018 (*PLOS Medicine*, variable generalization of pneumonia detection).

---
# 19. Example Exam with Answers (`Example_Exam_with_Answers.pdf`, 5 pages)

*"Example Exam — Medical Image Analysis." All questions have equal weight. Below, each question is summarized with its given solution.*

**Q1 — Field of view / voxel arithmetic for 2D acquisitions.** Background given: in 2D acquisitions, slices are acquired one after another in a predefined direction and stacked into a 3D volume (axial → slices in the transverse plane; coronal → slices in the coronal plane). Voxel sizes are given as *in-plane* (two values) and *through-plane* (one value — slice thickness).

- (a) Axial acquisition, in-plane 0.6 × 0.6 mm² with 320 pixels in both directions, through-plane 3.5 mm with 150 slices. FOV in head-to-toe, left-to-right, anterior-to-posterior? → **525 × 192 × 192 mm³** (head-to-toe = 150 × 3.5; in-plane 320 × 0.6 = 192 each).
- (b) Sagittal acquisition, in-plane 0.75 × 0.75 mm² with 256 pixels both directions, through-plane 2.5 mm with 210 slices. → **192 × 525 × 192 mm³** (left-to-right is the through-plane direction: 210 × 2.5 = 525; in-plane 256 × 0.75 = 192).
- (c) Coronal acquisition, in-plane 1.0 × 1.0 mm² with a 192 mm FOV in both directions, through-plane 2.5 mm with 275 mm FOV. Number of pixels in head-to-toe / left-to-right / anterior-to-posterior? → **192 × 192 × 110 voxels** (anterior-to-posterior is through-plane: 275/2.5 = 110).

**Q2 — MAP estimation for segmentation.** Given image $I$ and segmentation $S$, which maximization is the Maximum-A-Posteriori estimate: $\max_S \log p(I|S)$ or $\max_S \log[p(I|S)\,p(S)]$? → **The right one**, $\max_S \log[p(I|S)p(S)]$ (likelihood × prior).

**Q3 — Probabilistic vs. energy-based formulations.** Match terms between $\max_S \log p(I|S) + \log p(S)$ and $\min_S D(I,S) - \lambda R(S)$ *(as printed)*. → **$\log p(I|S) \leftrightarrow D(I,S)$** (data term) and **$\log p(S) \leftrightarrow \lambda R(S)$** (regularization/prior).

**Q4 — True/False on intensity normalization.**

- (a) Nyul's intensity normalization is a non-linear method → **TRUE** (piecewise linear overall = non-linear).
- (b) Min-max normalization using the 0 and 100 percentiles is particularly robust to intensity outliers → **FALSE** (0/100 percentiles are the min/max themselves — maximally outlier-sensitive).
- (c) Matching mean and standard deviation of two images can remove intensity differences due to bias fields in MRI → **FALSE** (bias field is a spatially varying multiplicative effect; a global linear intensity map cannot remove it).
- (d) With two pathologies, one hyper- and one hypo-intense, their effects would "cancel out" in min-max normalization with zero net adverse effect when matching to a healthy subject's profile → **FALSE** (both extremes distort min and max; they don't cancel).

**Q5 — GMM parameter count.** For an image with $N$ pixels and a single intensity per pixel, how many parameters does a 4-component Gaussian mixture model have? → **4 × 3 = 12** (per component: mean, variance, and mixture weight).

**Q6 — The two steps of EM (in words).** → The **Expectation step** computes posterior distributions given the model parameters — i.e., **soft class assignments**; the **Maximization step** optimizes the model parameters given the current soft class assignments.

**Q7 — K-means vs. EM-GMM: which statement is inaccurate?** (A) EM-GMM uses probabilistic (soft) assignments while K-means uses deterministic (hard) ones — accurate; (B) EM-GMM estimates means *and* covariances while K-means only uses means — accurate; (C) EM-GMM does not share K-means' sensitivity to the user-set number of components — **inaccurate → answer C** (both are sensitive to the number of components); (D) initialization is important for both — accurate.

**Q8 — Historical evolution of ML for medical images: which statement is incorrect?** (A) Univariate statistical models used hand-crafted, often difficult-to-extract features to identify statistical relations — correct; (B) multivariate predictive models (SVM, logistic regression) *reduced the feature-extraction effort* compared to univariate models by combining multiple hand-crafted features — **incorrect → answer B** (they still rely on the same hand-crafted features; combining more features does not reduce the extraction effort); (C) models with internal feature selection (random forests) could use lots of easy-to-extract primitive features — correct; (D) multi-layer neural networks automatically extract features from raw data at the cost of more parameters — correct.

**Q9 — Which algorithm uses no context at all for a pixel's class assignment?** (Context illustration: a pixel is much easier to segment when surrounding image information is available.) (A) Expectation-Maximization with pixel-wise Gaussian mixture models; (B) atlas-based segmentation; (C) random forest; (D) CNNs. → **Answer A** (pixel-wise GMM-EM treats each pixel's intensity independently; atlases bring spatial context, and RF/CNN features use neighborhoods).

**Q10 — Logistic regression vs. a single neuron with sigmoid activation?** Options: no bias term in logistic regression / one can model more complex relations than the other / only logistic regression is probabilistic / no difference. → **Answer E: there is no difference.**

**Q11 — LeNet-5 True/False (convolutional part only).** Architecture given: input 32×32 grayscale; Conv1: 6 kernels 5×5, stride 1, no padding; Pool1: max-pool 2×2, stride 2; Conv2: 16 kernels 5×5, stride 1, no padding; Pool2: max-pool 2×2, stride 2; then FC layers 120 → 84 → 10 outputs (10 digits).

- (a) The global receptive field of a neuron in Pool2 is 16×16 → **TRUE** (5 → 6 after pool → 14 after Conv2 → 16 after Pool2).
- (b) The global stride of a neuron in Pool2 is 2×2 → **FALSE** (two stride-2 poolings compound to a global stride of 4×4).
- (c) The total number of convolutional kernel weights (excluding biases) in Conv1+Pool1+Conv2+Pool2 is $5\cdot5\cdot1\cdot6 + 5\cdot5\cdot6\cdot16 = 2550$ → **TRUE** (pooling has no weights).
- (d) With 64×64 inputs instead of 32×32, the number of convolutional kernel weights in (c) would be 4× larger → **FALSE** (convolutional weight counts are independent of image size — that's the point of weight sharing).

**Q12 — Advantages of convolutional over fully connected layers (multiple answers).** (A) a single conv layer has a much larger receptive field — false (it's *smaller*/local); (B) conv layers can drastically reduce the number of parameters per layer — **correct**; (C) only conv layers allow advanced activations like ReLU — false; (D) multi-channel color images can only be processed convolutionally — false; (E) with conv layers the parameter count scales much better with increasing image size — **correct**. → **Answers: B and E.**

---

*End of notes — all 18 PDFs and the HTML demo in the repository have been processed.*
