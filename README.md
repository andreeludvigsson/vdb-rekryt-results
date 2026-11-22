# Rekryt: wall detection for house floor plans

![Beats CubiCasa5K](https://img.shields.io/badge/vs%20CubiCasa5K-better%20on%20102%20of%20122%20real%20plans-2e7d32)
![F1 CVC-FP](https://img.shields.io/badge/F1%20real%20scanned%20plans-0.881-2a78d6)
![F1 VDB drawings](https://img.shields.io/badge/F1%20VDB%20drawings-0.971-2a78d6)
![Training data](https://img.shields.io/badge/training%20data-commercial%20use%20OK-555555)

Rekryt is the wall detector in VDB, a Swedish house-design CAD application. You upload a floor-plan image, Rekryt finds
the walls, and VDB turns them into an editable drawing. The AI architect can then change that drawing on request.

**Training data:** Rekryt is trained from scratch on openly licensed data
([Swiss Dwellings](https://zenodo.org/records/7788422) by Archilyse AG, CC BY 4.0) and VDB's own synthetic drawings.

This repository holds results only. The model, the training data and the application code are not published.

## Rekryt vs CubiCasa5K on real scanned floor plans

Measured on the **CVC-FP** dataset: 122 real, scanned floor plans with hand-made wall ground truth. Rekryt had never seen
this dataset before it was measured.

| Model | F1 mean | F1 median | Worst plan | Best plan |
|---|---:|---:|---:|---:|
| **Rekryt v5** | **0.881** | **0.886** | 0.573 | **0.951** |
| CubiCasa5K (official pretrained model) | 0.855 | 0.858 | 0.748 | 0.927 |

**Rekryt is better on 102 of the 122 plans.** The difference in mean F1 is **+0.026** (95 % confidence interval
+0.018 to +0.034, paired bootstrap over the 122 plans).

### Where Rekryt wins most

The six plans with Rekryt's largest lead over CubiCasa5K. Panels: **original | ground truth | CubiCasa5K | Rekryt v5**.
Green is the dataset's own ground truth, red is CubiCasa5K, blue is Rekryt.

![CVC-FP image003](images/cvc_image003.png)

![CVC-FP image014](images/cvc_image014.png)

![CVC-FP image008](images/cvc_image008.png)

![CVC-FP IIa_SR1102](images/cvc_IIa_SR1102.png)

![CVC-FP 7](images/cvc_7.png)

![CVC-FP IIa_GC1202](images/cvc_IIa_GC1202.png)

Images and ground truth: CVC-FP (de las Heras et al., IJDAR 2015), CC BY-NC 4.0.

## VDB's own drawings

57 plans exported from VDB itself: 17 start houses and 40 generated houses. The ground truth comes straight from the
drawing's own geometry. The whole sheet goes in as an uploaded drawing would, frame and title block included.

| Model | Precision | Recall | F1 mean | F1 median | F1 min–max |
|---|---:|---:|---:|---:|---:|
| **Rekryt v5** | **0.948** | **0.997** | **0.971** | **0.975** | **0.937**–**0.992** |
| CubiCasa5K | 0.942 | 0.996 | 0.968 | 0.970 | 0.915–0.986 |

Rekryt is better on 37 of the 57 drawings (+0.004 mean F1, 95 % CI +0.001 to +0.007).

## How it is measured

- **Recall** is the share of ground-truth wall pixels a model finds. **Precision** is the share of a model's wall pixels
  that lie on a real wall. A pixel counts as a hit within max(2 px, 0.4 % of the image's longest side). F1 combines both.
- CubiCasa5K is the official pretrained model (Kalervo et al., 2019), run with the authors' own test procedure and the
  better of two input sizes.
- The same ground truth, tolerance and plans are used for both models.
- Houses kept for final evaluation are not shown here.

## Sources and licences

- CVC-FP: de las Heras, Terrades, Robles, Sánchez, "CVC-FP and SGT: a new database for structural floor plan analysis
  and its groundtruthing tool", IJDAR 2015. CC BY-NC 4.0; its plans and ground truth are shown above for comparison.
- CubiCasa5K: Kalervo, Ylioinas, Häikiö, Karhu, Kannala, "CubiCasa5K: A Dataset and an Improved Multi-Task Model for
  Floorplan Image Analysis", SCIA 2019. Model and weights CC BY-NC 4.0, used here only for comparison; neither is part of VDB.
- Swiss Dwellings: Archilyse AG, CC BY 4.0.

## Related

- [vdb-amd-results](https://github.com/andreeludvigsson/vdb-amd-results): how VDB's AI architect was trained on an AMD
  Instinct MI300X.
- **One step at a time: Sweden first, then the Nordics, then Europe, then the world.**

---

Results as of 2026-11-22.
