---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
slug: {{ .Name }}
draft: true 
categories:
- All
tags:
- Tag
comments: true
ShowToc: true

# cover:
#   image: "run-python.gif"
#   alt: "Running a python script inside a kali docker container running on Windows."
#   relative: true
---