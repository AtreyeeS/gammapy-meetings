# Comments from the CTA-IRF group

## Summarised from:
1. Feedback from the Science Working Groups 
https://forge.in2p3.fr/projects/instrument-response-functions/wiki/Discussion_on_SWG_feedback_with_IRF_production_and_Science_tools_experts

2. Discussions on IRF format evaluation
https://forge.in2p3.fr/projects/instrument-response-functions/wiki/Current_IRF_format_evaluation

## Implications on gammapy:

1. **Point-like IRFs**
-  If required, CTA may supply *point-like IRFs* (TBD). These will be defined at the nodes, not centers.
The data format and science tools must support both and adapt accordingly - `node` centering in IRF builing. 
- Who decides which IRF to choose? Can the science tools do it automatically? - Adaptations in `DataStore`?

2. **GTI**
- KK:  "the current view is that IRFs will be associated with standard GTIs chosen by instrumental effects, 
however the user has the freedom to sub-divide a given GTI into smaller parts based on the instrument monitoring table provided
with the event list for example."
- Already possible within gammapy

3. **Non-radially symmetric (about the FoV) IRFs **
- KK: "We almost certainly always will need non-radially symmetric (about the FOV center)  IRFs (PSF, Aeff, etc), since with a large FOV, 
the zenith angle across the field of view will vary significantly, 
and thus the response across the FOV will have a preferred axis and a radially symmetric IRF will not be enough.
Therefore, I think the IRF format must support variance in r and phi (or det_x, det_y, though it is much better/smaller stored in radial coordinates). 
Even if we define the phi variance as a single bin to start with, it's important to add this."
- Currently not supported by GADF - need to come up with a better model
- Do we need an internal data model within gammapy - we need to have a more generic IRF structure that supports arbitrary number of axes.
- perform FoV rotation in a consistent manner.
- IRF maps with more axes?

4. **Non-isotropic PSF (about the source position)**
- KK: "We probably will need non-radially  symmetric (about the source position) PSFs at least in the construction /early science phase, 
since we will likely have periods where the array and PSF is very non-symmetric"
- Currently not supported within GADF.
- How do we support this within gammapy?
- this we can support with the PSFMap by adding a rad-y axis. This comes at the cost of a heavy load on RAM. 
Typically one such matrix will be ~1Gb. This means that we will really need multiresolution map.

5. *Including event types*
- Each event will problably have its own `event_type` tag (eg: depending on the psf)
- Does this imply each event has a different IRF?

6. Including uncertainties on the IRFs
- Including uncertainties on the IRFs should be the job of science tools.
- Possibly need to distinguish between systematic and statistical uncertainties.
- Doing it a part of every analysis is probably too heavy. 
- Guidelines for bracketing : https://docs.google.com/document/d/1oBOwOOgMcL8Shww6oLjiQoQVbOwl0HGuBTV1YGHTc_k/edit
