---
layout: splash
permalink: /

title: "![image-right](/assets/images/mHM_textlogo.png){:.align-left}"
excerpt: "Start using it today:<br>[`conda install mhm`](/guides/)"

header:
  og_image: /assets/images/mHM_textlogo.png
  overlay_color: "#333"
  overlay_filter: "0.5"
  overlay_image: /assets/images/banner.png
  caption: "Image credit: [**Luis E. Samaniego**](https://www.ufz.de/index.php?en=38094)"
  actions:
    - label: "<i class='fab fa-gitlab'></i> Documentation"
      url: "/docs/"
    - label: "<i class='fab fa-gitlab'></i> Source Code"
      url: "https://git.ufz.de/mhm/mhm/"
      btn_class: "btn--success"

feature_row_1:
  - image_path: /assets/images/previews/paper.jpeg
    alt: "Publications"
    title: "High Reputation"
    excerpt: "mHM is a well established hydrological model used by the scientific community."
    url: "/about/publications/"
    btn_class: "btn--primary"
    btn_label: "Learn more"
  - image_path: /assets/images/previews/dev.png
    alt: "Development"
    title: "Active Development"
    excerpt: "mHM is under active development with dozens of contributors."
    url: "/about/releases/"
    btn_class: "btn--primary"
    btn_label: "Learn more"
  - image_path: /assets/images/previews/meet.png
    alt: "Community"
    title: "Wholesome Community"
    excerpt: "mHM has an open community always interested in discussions."
    url: "https://github.com/mhm-ufz/mhm/discussions"
    btn_class: "btn--primary"
    btn_label: "Learn more"
---

{% include feature_row id="feature_row_1" %}

# Latest News
{: .text-center}
<div class="grid__wrapper">
  {% for post in site.posts limit:4 %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
