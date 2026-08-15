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
