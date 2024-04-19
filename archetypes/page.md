---
title: "{{ replace .Name "-" " " | title }}"
description: ""
date: {{ .Date }}
lastmod: {{ .Date }}
aliases:
  - about-us
  - about-hugo
  - contact
license: CC BY-NC-ND
menu:
    main: 
        weight: -90
        params:
            icon: user
comments: false
image: ''
---