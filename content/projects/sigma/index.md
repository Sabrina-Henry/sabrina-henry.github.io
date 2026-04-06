---
title: "SIGMA: Self-Guided Integrated Gradient Method for Attribution"
date: 2026-01-01
summary: "A feature attribution method that follows a models decision boundary to explain deep neural network predictions."
tags:
  - Explainable AI
  - Feature Attribution
  - Computer Vision
links:
  - type: github
    url: https://github.com/HWQuantum/SIGMA
    label: Code
  - type: custom
    url: https://www.techrxiv.org/doi/full/10.36227/techrxiv.175037106.61341481
    label: Preprint
---

Modern neural networks can have very high confidence — but confidence alone does not tell us *why* a prediction was made.

Feature attribution methods aim to answer this *why* by highlighting which parts of an input most influenced a model's decision. In practice, however, the way we choose to explain a prediction can actually shape the explanation itself.

## What does it mean to explain a prediction?

Given an input and a trained model, an attribution method assigns importance scores to individual input features — pixels in an image, time points on a graph, or tokens in text — indicating how strongly each contributed to the final prediction.

Among the most widely used approaches are *path-based* methods, which accumulate model gradients as the input is gradually transformed, telling us how sensitive each input feature is to the output prediction.

{{< rawhtml >}}
<div class="interactive-figure">
  <div class="figure-frame">
    <img src="Interactive_figs/ladybug.png" alt="Input image" class="figure-image base-image">
    <img src="Interactive_figs/SIGMA_ladybug.png" alt="Integrated Gradients attribution" class="figure-image attribution-image">
  </div>
  <button class="toggle-btn" onclick="toggleAttribution(this)">Click to Show Attribution</button>
  <figcaption>The model predicted the class 'Ladybug' for this input image, click to highlight which pixels were most important in this prediction.</figcaption>
</div>
{{< /rawhtml >}}

## The baseline assumption

Integrated Gradients explains a prediction by integrating gradients along a straight path from a *baseline* input to the image being explained. The baseline is intended to represent the absence of information — often a black or blurred image. However, different baselines can lead to noticeably different explanations, even when the model and input remain unchanged.

{{< rawhtml >}}
<figure class="interactive-figure" data-figure="baseline-1">
  <div class="figure-frame-overlay">
    <img src="Interactive_figs/ladybug.png" alt="Input image" class="figure-image base-image">
    <img src="" alt="Attribution overlay" class="figure-image attribution-image" data-overlay>
  </div>
  <div class="toggle-row" role="group" aria-label="Choose baseline">
    <button class="toggle-btn baseline-btn" aria-pressed="true" data-src="Interactive_figs/IG_black_baseline.png" onclick="setBaseline(this)">Black</button>
    <button class="toggle-btn baseline-btn" aria-pressed="false" data-src="Interactive_figs/IG_noise_baseline.png" onclick="setBaseline(this)">Noise</button>
    <button class="toggle-btn baseline-btn" aria-pressed="false" data-src="Interactive_figs/IG_blur_baseline.png" onclick="setBaseline(this)">Blurred</button>
  </div>
  <figcaption>Integrated Gradients attribution for the same input using different baselines.</figcaption>
</figure>
{{< /rawhtml >}}

## Saturation along attribution paths

As the input moves away from the baseline, the model's confidence often increases rapidly before entering a saturated region where further changes have little effect on the output. Gradients accumulated in these flat regions can dominate the final attribution, despite contributing little to the actual decision.

{{< rawhtml >}}
<figure class="interactive-figure figure-large">
  <div class="figure-frame">
    <img src="Interactive_figs/saturation-01.png" alt="Saturation along attribution paths" class="figure-image">
  </div>
</figure>
{{< /rawhtml >}}

## Thinking in terms of confidence landscapes

Rather than viewing attribution as interpolation between two images, it can be helpful to think of the model's output as a *confidence landscape* over input space. An informative explanation should focus on the regions where the model's belief actually changes — not where it is already certain.

{{< rawhtml >}}
<figure class="interactive-figure figure-large">
  <div class="figure-frame">
    <img src="Interactive_figs/conflandscape.png" alt="Confidence landscape" class="figure-image">
  </div>
</figure>
{{< /rawhtml >}}

## A self-guided path

SIGMA removes the need for a baseline entirely. Instead of following a predefined path, SIGMA constructs its own trajectory by iteratively perturbing the input in directions that reduce the model's confidence in the predicted class. Gradients are accumulated along this path and weighted by the corresponding drop in confidence.

{{< rawhtml >}}
<figure class="interactive-figure figure-video">
  <div class="figure-frame">
    <video class="article-video" autoplay muted loop playsinline controls preload="auto">
      <source src="Interactive_figs/sigma_side_by_side.mp4" type="video/mp4">
      Sorry — your browser can't play this video.
    </video>
  </div>
  <figcaption>SIGMA trajectory through a prediction landscape with corresponding perturbed input and attribution evolution.</figcaption>
</figure>

<figure class="interactive-figure figure-method">
  <div class="figure-frame">
    <img src="Interactive_figs/SIGMA.png" alt="SIGMA attribution method" class="figure-image">
  </div>
</figure>
{{< /rawhtml >}}

## What changes in practice?

Because SIGMA follows the model's confidence downhill, it avoids early saturation and continues to collect informative gradients throughout the path. The resulting attribution maps tend to be more spatially coherent and less influenced by regions that have little effect on the prediction.

{{< rawhtml >}}
<figure class="interactive-figure figure-large">
  <div class="figure-frame">
    <img src="Interactive_figs/further_results_qualitative.png" alt="Further results" class="figure-image">
  </div>
</figure>
{{< /rawhtml >}}

## Faithfulness to model behaviour

By accumulating gradients in proportion to confidence change, SIGMA aligns attribution strength with the model's sensitivity. Revealing input features in order of attribution importance reconstructs the model's confidence more efficiently than random or saturated-gradient explanations.

## Beyond explanation

Following confidence collapse naturally produces perturbed inputs that remain recognisable to humans but receive near-zero confidence from the model. These low-confidence variants expose decision boundaries and can be reused as training augmentations, linking interpretability and model reliability.

{{< rawhtml >}}
<div class="perturb-widget">
  <div class="perturb-buttons" role="tablist" aria-label="Choose perturbation">
    <button class="perturb-btn is-active" data-mode="clean" aria-selected="true">
      <img src="Interactive_figs/clean_button.png" alt="Clean example">
    </button>
    <button class="perturb-btn" data-mode="gaussian" aria-selected="false">
      <img src="Interactive_figs/gaussian_button.png" alt="Gaussian example">
    </button>
    <button class="perturb-btn" data-mode="fgsm" aria-selected="false">
      <img src="Interactive_figs/fgsm_button.png" alt="FGSM example">
    </button>
    <button class="perturb-btn" data-mode="sigma" aria-selected="false">
      <img src="Interactive_figs/sigma_button.png" alt="SIGMA example">
    </button>
  </div>
  <figure class="chart-figure">
    <img id="chartImg" src="Interactive_figs/web_bar_original.png" alt="Bar chart for Clean perturbation">
    <figcaption id="chartCap">Original network performance (Clean)</figcaption>
  </figure>
</div>

<script>
(function () {
  const CHARTS = {
    clean:    { src: "Interactive_figs/web_bar_original.png", cap: "Original network performance (Clean)" },
    gaussian: { src: "Interactive_figs/web_bar_gaussian.png", cap: "Network performance under Gaussian noise augmentations" },
    fgsm:     { src: "Interactive_figs/web_bar_FGSM.png", cap: "Network performance under FGSM augmentations" },
    sigma:    { src: "Interactive_figs/web_bar_SIGMA.png", cap: "Network performance under SIGMA augmentations" }
  };
  const chartImg = document.getElementById("chartImg");
  const chartCap = document.getElementById("chartCap");
  function setActive(btn) {
    document.querySelectorAll(".perturb-btn").forEach(b => {
      const active = (b === btn);
      b.classList.toggle("is-active", active);
      b.setAttribute("aria-selected", active ? "true" : "false");
    });
  }
  function swapChart(mode) {
    const d = CHARTS[mode];
    chartImg.src = d.src;
    chartImg.alt = "Bar chart for " + mode;
    chartCap.textContent = d.cap;
  }
  document.querySelectorAll(".perturb-btn").forEach(btn => {
    btn.addEventListener("click", () => {
      setActive(btn);
      swapChart(btn.dataset.mode);
    });
  });
})();
</script>
{{< /rawhtml >}}

## Conclusion

SIGMA offers a shift in perspective for attribution based interpretability — from explaining predictions relative to arbitrary references, to explaining them by following the model's own confidence. By treating explanations as paths shaped by the model itself, we gain both clearer attributions and a deeper understanding of how confidence emerges and collapses.

{{< rawhtml >}}
<script>
function toggleAttribution(button) {
  const figure = button.closest(".interactive-figure");
  if (!figure) return;
  const attribution = figure.querySelector(".attribution-image");
  if (!attribution) return;
  const isVisible = attribution.classList.toggle("visible");
  button.textContent = isVisible ? "Hide attribution" : "Show attribution";
  button.setAttribute("aria-pressed", isVisible ? "true" : "false");
}

function setBaseline(button) {
  const figure = button.closest(".interactive-figure");
  if (!figure) return;
  const overlay = figure.querySelector("img[data-overlay]");
  if (!overlay) return;
  const buttons = figure.querySelectorAll(".baseline-btn");
  buttons.forEach(btn => {
    const isActive = btn === button;
    btn.classList.toggle("active", isActive);
    btn.setAttribute("aria-pressed", isActive ? "true" : "false");
  });
  const src = button.getAttribute("data-src").trim();
  overlay.src = src;
  overlay.classList.add("visible");
}

document.addEventListener("DOMContentLoaded", () => {
  document.querySelectorAll(".interactive-figure").forEach(fig => {
    const overlay = fig.querySelector("img[data-overlay]");
    const active = fig.querySelector(".baseline-btn.active");
    if (overlay && active) setBaseline(active);
  });
});
</script>
{{< /rawhtml >}}

---

**Paper:** [Preprint on TechRxiv](https://www.techrxiv.org/doi/full/10.36227/techrxiv.175037106.61341481)  
**Code:** [GitHub repository](https://github.com/HWQuantum/SIGMA)  
**Contact:** [sjh9@hw.ac.uk](mailto:sjh9@hw.ac.uk)



