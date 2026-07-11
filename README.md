import pygame
import math
import random

pygame.init()

WIDTH, HEIGHT = 800, 800
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Radar Visualization")

GREEN = (0, 255, 0)
BLACK = (0, 0, 0)

CENTER = (WIDTH // 2, HEIGHT // 2)
RADIUS = 350

clock = pygame.time.Clock()
angle = 0

targets = []

for _ in range(15):
    a = random.uniform(0, 360)
    d = random.uniform(50, RADIUS)
    targets.append((a, d))

running = True

while running:
    clock.tick(60)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    screen.fill(BLACK)

    # Radar circles
    for r in [70, 140, 210, 280, 350]:
        pygame.draw.circle(screen, GREEN, CENTER, r, 1)

    # Cross lines
    pygame.draw.line(screen, GREEN, (CENTER[0]-RADIUS, CENTER[1]),
                     (CENTER[0]+RADIUS, CENTER[1]), 1)
    pygame.draw.line(screen, GREEN, (CENTER[0], CENTER[1]-RADIUS),
                     (CENTER[0], CENTER[1]+RADIUS), 1)

    # Sweep line
    x = CENTER[0] + RADIUS * math.cos(math.radians(angle))
    y = CENTER[1] - RADIUS * math.sin(math.radians(angle))
    pygame.draw.line(screen, GREEN, CENTER, (x, y), 2)

    # Targets
    for ta, td in targets:
        tx = CENTER[0] + td * math.cos(math.radians(ta))
        ty = CENTER[1] - td * math.sin(math.radians(ta))

        diff = abs((angle - ta + 180) % 360 - 180)

        if diff < 5:
            pygame.draw.circle(screen, GREEN, (int(tx), int(ty)), 6)
        else:
            pygame.draw.circle(screen, (0, 80, 0), (int(tx), int(ty)), 4)

    angle = (angle + 1) % 360

    pygame.display.flip()

pygame.quit()
