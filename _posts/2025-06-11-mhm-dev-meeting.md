---
title:  "mHM Developer meeting June 11th 2025"
excerpt: The June 2025 mHM developer meeting.
header:
  teaser: /assets/posts/2021-06-11/teaser.png

author: "Stephan Thober"
---

{% include figure image_path="/assets/posts/2025-06-11/mHM_developers.png" alt="mHM developers." caption="mHM developers taking part in the June 2025 mHM developer meeting." %}

# mHM Developer Meeting June 12th 2025 Recap

After a long hiatus, we are very happy to share impressions of the first mHM Developer meeting in 2025. Various colleagues from UFZ attended and we discussed a diverse range of topics from plant transpiration, groundwater parametrization, and river network representation. 

On the soil–plant interface, multiple contributors are refining how vegetation interacts with soil moisture. Sven Westermann (https://www.ecohydrology.uni-jena.de/40/sven-westermann) presented his work on the impact of root fraction within mHM. We discussed the combined effect with changes in soil water stress functions that have been recently looked into by Friedrich Boeing (https://www.ufz.de/index.php?en=47624). 

Another crucial topic that we touched is the interaction between the unsaturated and saturated sub-surface, or in other words how groundwater representation can be improved in mHM. Afid Nur Kholis presented his latest development about introducing a water table depth in mHM allowing a physically more consistent coupling to 3d groundwater models like OGS (https://www.opengeosys.org). This provides an alternative representation to the classical cascade of linear storages.

Last but not least, Sebastian Müller (https://www.ufz.de/index.php?en=38073) presented his latest updated on the use of directed acyclic graphs for river network representation. This allows for a more efficient parallelization of the routing process and in turn to a tremendeous speed-up of mHM simulations.

We will continue to discuss these and more topics further in the future developer meetings. We are also looking forward to restart the mHM user meeting in the near future.
