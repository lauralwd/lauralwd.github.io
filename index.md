---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: splash
author_profile: false
classes: wide
header:
  overlay_image: /assets/images/Azolla_wide_green-red2.jpg
  caption: "_Azolla filiculoides_ floating on water" 

feature_row_about:
  - image_path: assets/images/bio-photo2.jpg
    alt: "Me"
    title: "About me"
    excerpt: "Hi there and welcome to my portfolio page (under construction still). I'm a PhD candidate at Utrecht University where I study the metagenome the amazing fern _Azolla_! On this page, I keep track of my academic projects, and some personal ones too."
    url: "/about/"
    btn_label: "Read more about me"
feature_row_projects:
  - image_path: assets/images/nijmegen_sampling.jpg
    alt: "me sampling"
    title: "Science"
    excerpt: "Read more about my scientific projects."
    url: "/science/"
    btn_label: "Read more"
  - image_path: /assets/images/kennis-van-nu_interview.jpg
    image_caption: "a caption"
    alt: "me in an interview"
    title: "Outreach"
    excerpt: "outreach blurb"
    url: "/outreach/"
    btn_label: "Read more"
  - image_path: assets/images/elementary_school_breeding_lecture.jpg
    title: "Teaching"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "/teaching/"
    btn_label: "Read more"
  - image_path: assets/images/oscu_symposium.jpg
    title: "Open Science"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "/openscience/"
    btn_label: "Read more"
  - image_path: assets/images/management_OS_uu-header.jpg
    title: "Management"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "/Management/"
    btn_label: "Read more"
  - image_path: assets/images/schotland_camera.jpg
    title: "Photography"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "/photography/"
    btn_label: "See more"
  - image_path: assets/images/3dprinter_disassembled.jpg
    title: "Technology"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "/technology/"
    btn_label: "Read more"

---

{% include feature_row id="intro" type="center" %}

{% include feature_row id="feature_row_about" type="left" %}

{% include feature_row id="feature_row_projects" %}


