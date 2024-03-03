import pygame
import random
import sys

# Initialize Pygame
pygame.init()

# Set up the screen
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Geometry Dash")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Player
player_width, player_height = 50, 50
player_x, player_y = 50, HEIGHT - player_height - 50
player_vel = 5
player_jump = False
jump_count = 10

# Obstacles
obstacle_width, obstacle_height = 50, 50
obstacle_x = WIDTH
obstacle_y = HEIGHT - obstacle_height - 50
obstacle_vel = 5

# Scoring
score = 0
font = pygame.font.Font(None, 36)

# Game Loop
clock = pygame.time.Clock()

running = True
while running:
    screen.fill(BLACK)

    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE and not player_jump:
                player_jump = True

    # Player movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_SPACE] and not player_jump:  
        player_jump = True

    if player_jump:
        if jump_count >= -10:
            neg = 1
            if jump_count < 0:
                neg = -1
            player_y -= (jump_count ** 2) * 0.5 * neg
            jump_count -= 1
        else:
            player_jump = False
            jump_count = 10

    # Obstacle movement
    obstacle_x -= obstacle_vel
    if obstacle_x <= 0:
        obstacle_x = WIDTH
        obstacle_y = random.randint(50, HEIGHT - obstacle_height - 50)
        score += 1

    # Collision detection
    if player_x < obstacle_x + obstacle_width and player_x + player_width > obstacle_x:
        if player_y < obstacle_y + obstacle_height and player_y + player_height > obstacle_y:
            print("Collision!")
            # You can add game over logic here

    # Draw player and obstacle
    pygame.draw.rect(screen, WHITE, (player_x, player_y, player_width, player_height))
    pygame.draw.rect(screen, RED, (obstacle_x, obstacle_y, obstacle_width, obstacle_height))

    # Display score
    score_text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(score_text, (10, 10))

    pygame.display.update()
    clock.tick(60)  # FPS

pygame.quit()
sys.exit()

