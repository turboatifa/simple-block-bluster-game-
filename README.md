# 🎮 Block Blaster

A simple **Block Blaster Game** made with Python and Pygame.

## 🚀 Features
- Move paddle with **← → arrow keys**
- Bounce the ball to break blocks
- Gain **+10 points** for each block
- Game over when the ball touches bottom

## 🛠 Requirements
- Python 3.x
- Pygame

Install dependencies:
```bash
pip install pygame

import pygame
import random
import sys

# Initialize pygame
pygame.init()

# Screen settings
WIDTH, HEIGHT = 600, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Block Blaster by Atifa")

# Colors
WHITE = (255, 255, 255)
RED = (200, 50, 50)
BLUE = (50, 50, 200)
BLACK = (0, 0, 0)

# Player (paddle)
paddle_width, paddle_height = 100, 15
paddle = pygame.Rect(WIDTH//2 - paddle_width//2, HEIGHT - 40, paddle_width, paddle_height)

# Ball
ball = pygame.Rect(WIDTH//2, HEIGHT//2, 15, 15)
ball_speed = [4, -4]

# Blocks
blocks = []
block_rows, block_cols = 5, 8
block_width, block_height = 60, 20
for row in range(block_rows):
    for col in range(block_cols):
        block_x = col * (block_width + 10) + 35
        block_y = row * (block_height + 10) + 30
        blocks.append(pygame.Rect(block_x, block_y, block_width, block_height))

# Game loop
clock = pygame.time.Clock()
running = True
score = 0

while running:
    screen.fill(BLACK)
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    
    # Paddle movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and paddle.left > 0:
        paddle.move_ip(-6, 0)
    if keys[pygame.K_RIGHT] and paddle.right < WIDTH:
        paddle.move_ip(6, 0)
    
    # Ball movement
    ball.move_ip(ball_speed)
    
    # Ball collisions
    if ball.left <= 0 or ball.right >= WIDTH:
        ball_speed[0] = -ball_speed[0]
    if ball.top <= 0:
        ball_speed[1] = -ball_speed[1]
    if ball.colliderect(paddle):
        ball_speed[1] = -ball_speed[1]
    
    # Ball hits blocks
    hit_index = ball.collidelist(blocks)
    if hit_index != -1:
        del blocks[hit_index]
        ball_speed[1] = -ball_speed[1]
        score += 10
    
    # Lose condition
    if ball.bottom >= HEIGHT:
        print("Game Over! Final Score:", score)
        running = False
    
    # Draw
    pygame.draw.rect(screen, BLUE, paddle)
    pygame.draw.ellipse(screen, RED, ball)
    for block in blocks:
        pygame.draw.rect(screen, WHITE, block)
    
    # Score
    font = pygame.font.SysFont(None, 30)
    text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(text, (10, 10))
    
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
