import pygame
import random

pygame.init()

WIDTH, HEIGHT = 500, 500
CELL = 20

screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Змейка")

clock = pygame.time.Clock()

# змейка
snake = [(100, 100)]
dx, dy = CELL, 0

# еда
food = (random.randrange(0, WIDTH, CELL), random.randrange(0, HEIGHT, CELL))

def draw():
    screen.fill((0, 0, 0))

    # еда
    pygame.draw.rect(screen, (255, 0, 0), (*food, CELL, CELL))

    # змейка
    for s in snake:
        pygame.draw.rect(screen, (0, 255, 0), (*s, CELL, CELL))

    pygame.display.flip()

running = True

while running:
    clock.tick(10)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    if keys[pygame.K_UP] and dy == 0:
        dx, dy = 0, -CELL
    if keys[pygame.K_DOWN] and dy == 0:
        dx, dy = 0, CELL
    if keys[pygame.K_LEFT] and dx == 0:
        dx, dy = -CELL, 0
    if keys[pygame.K_RIGHT] and dx == 0:
        dx, dy = CELL, 0

    # движение
    head = (snake[0][0] + dx, snake[0][1] + dy)

    # проверка столкновений
    if (head in snake or
        head[0] < 0 or head[0] >= WIDTH or
        head[1] < 0 or head[1] >= HEIGHT):
        print("Game Over")
        running = False

    snake.insert(0, head)

    # еда
    if head == food:
        food = (random.randrange(0, WIDTH, CELL),
                random.randrange(0, HEIGHT, CELL))
    else:
        snake.pop()

    draw()

pygame.quit()
