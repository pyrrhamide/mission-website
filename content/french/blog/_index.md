---
title: Blog
description: |
  This is a fully featured blog that supports categories, 
  tags, series, and pagination.
author: "Kanto Fiaferana"
show_post_thumbnail: true
thumbnail_left: true # for list-sidebar only
show_author_byline: true
show_post_date: true
# for listing page layout
layout: list-sidebar # list, list-sidebar, list-grid

# for list-sidebar layout
sidebar: 
  title: Can't take it back once it's been set in motion
  description: |
    You know I need you for the oxytocin. 
  author: "Kanto Fiaferana"
  text_link_label: ""
  text_link_url: ""
  show_sidebar_adunit: true # show ad container

# set up common front matter for all pages inside blog/
cascade:
  author: "Kanto Fiaferana"
  show_author_byline: true
  show_post_date: true
  show_comments: true # see site config to choose Disqus or Utterances
  # for single-sidebar layout
  sidebar:
    text_link_label: View recent posts
    text_link_url: /blog/
    show_sidebar_adunit: false # show ad container
---
