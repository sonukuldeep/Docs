---
author: kuldeep
datetime: 2023-07-30
title: Pygame
slug: "pygame"
featured: false
draft: false
tags:
  - pygame
ogImage: ""
description: Codes for everything related to Pygame
---

![pygame](https://miro.medium.com/v2/resize:fit:1352/0*m0fEKM3DboA9qSlb.gif)

# Pygame

## Boilerplate

```py
import pygame
from sys import exit

pygame.init()
screen = pygame.display.set_mode((800, 400))
pygame.display.set_caption("Runner")
clock = pygame.time.Clock()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            exit()

    pygame.display.update()
    clock.tick(60)
```
