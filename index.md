---
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: splash
author_profile: false
classes: wide
header:
  overlay_image: /assets/images/Azolla_wide_green-red2.jpg
  caption: "_Azolla filiculoides_ floating on water" 
  overlay_color: "#000"
  overlay_filter: "0.3"

feature_row_present:
  - image_path: assets/images/azolla_anzali-macro-800x800.jpg
    alt: "Azolla anzali macro photo"
    title: "EPS get2gether 2022"
    image_caption: "Azolla anzali"
    excerpt: "At the Annual EPS get2gether, I'll present some aspects of my PhD project. My slides are available online during the meeting. Feel free to approach me at any time online or offline about the content of my talk. Partners & Passengers: endophytic microbes persistently present in the genus Azolla revealed by comparative metagenomics"
    url: "/present/"
    btn_label: "Go to slides"
    btn_class: "btn--success"

feature_row_about:
  - image_path: assets/images/bio-photo2.jpg
    alt: "Laura Dijkhuizen, young academic/scientist at Utrecht University"
    title: "About me"
    image_caption: "Yours truly"
    excerpt: "Hi there! I’m Laura -- a bioinformatics trainer and data‐driven researcher passionate about plant symbiosis and reproducible science. Here you’ll find my R&D projects, teaching materials, and outreach highlights. Feel free to explore and drop me a line if you’d like to collaborate or learn more!"
    url: "/about/"
    btn_label: "Read more about me"
    btn_class: "btn--info"
feature_row_projects:
  - image_path: assets/images/nijmegen_sampling.jpg
    alt: "Me sampling and making notes"
    image_caption: "Sampling and making notes"
    title: "Science"
    excerpt: "Read more about my scientific projects on the amazing fern _Azolla_. Scientific projects include metagenomics, fern physiology and phylogeny."
    url: "/science/"
    btn_class: "btn--info"
    btn_label: "Read more"
  - image_path: /assets/images/kennis-van-nu_interview.jpg
    image_caption: "Interview for 'NTR: De kennis van Nu"
    alt: "Me being interviewed by a camera crew"
    title: "Outreach"
    excerpt:  "The public funds my science; hence I’m a public servant. I try to help out in educative and outreach activities whenever I can to give back and communicate the science I'm involved in."
    url: "/outreach/"
    btn_class: "btn--info"
    btn_label: "Read more"
  - image_path: assets/images/elementary_school_breeding_lecture.jpg
    alt: "Me teaching a class in the local botanical gardens"
    image_caption: "Teaching a class in the botanical gardens"
    title: "Teaching"
    excerpt: "Academics are teachers as well as scientists. As a PhD, I have been granted extra time to work on my teaching skills. With this time, I aim to be certified with a 'basic teaching qualification' at the end of my PhD."
    url: "/teaching/"
    btn_class: "btn--info"
    btn_label: "Read more"
  # - image_path: assets/images/oscu_symposium.jpg
  #   alt: "A picture I took on the Open Science Symposium of UU's Science faculty in 2019"
  #   image_caption: "Open Science Symposium of UU's Science faculty"
  #   title: "Open Science"
  #   excerpt: "Being a young academic, I have fallen for the ways of open science. Here I document some of my attempts to make my science as open as I can."
  #   url: "/openscience/"
  #   btn_class: "btn--info"
  #   btn_label: "Read more"
  - image_path: assets/images/management_OS_uu-header-crop.jpg
    image_caption: "The rector magnificus signs the DORA declaration"
    alt: "The rector magnificus signs the DORA declaration"
    title: "Management"
    excerpt: "I am actively involved in several management bodies to represent fellow PhDs and practice my leadership skills. These management bodies include the Graduate School of Life Sciences and the Open Science programme, both here at UU."
    url: "/management/"
    btn_class: "btn--info"
    btn_label: "Read more"
  - image_path: assets/images/schotland_camera.jpg
    image_caption: "Taking a picture of the amazing Scottish landscape"
    alt: "Me taking a picture of the amazing Scottish landscape"
    title: "Photography"
    excerpt: "I love photography, as an art and as a science. Here are some examples of my photos, mostly macro's of plants and landscapes of travelling."
    url: "/photography/"
    btn_class: "btn--info"
    btn_label: "See more"
  # - image_path: assets/images/3dprinter-fiddeling-1.jpg
  #   image_caption: "Fiddeling with new circuitry for my 3D printer"
  #   alt: "Me fiddeling with new circuitry for my 3D printer"
  #   title: "Technology"
  #   excerpt: "Since I was a kid, I have always liked gadgets, computers and electronics. Here I talk more about 3D printing, Linux/BASH, and bioinformatics."
  #   url: "/technology/"
  #   btn_class: "btn--info"
  #   btn_label: "Read more"
  - image_path: assets/images/CV_snapshot.jpg
    image_caption: "My latest CV"
    alt: "Download my latest CV here"
    title: "Latest Curriculum Vitae"
    excerpt: "Like a true nerd, my CV is versioned and rendered via GitHub actions. Download it here! And fellow-nerds can find the source code [here](https://github.com/lauralwd/CV_generator)"
    url: "/CV_generator/"
    btn_class: "btn--success"
    btn_label: "Download CV"

---

{% include feature_row id="intro" type="center" %}

{% include feature_row id="feature_row_about" type="left" %}

{% include feature_row id="feature_row_projects" %}


