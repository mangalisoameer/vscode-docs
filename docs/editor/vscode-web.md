import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up the screen
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Mariyo Game")

# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Constants
PLAYER_WIDTH = 40
PLAYER_HEIGHT = 60
PLAYER_SPEED = 5
GRAVITY = 0.5
JUMP_HEIGHT = 10

# Player
player = pygame.Rect(50, HEIGHT - PLAYER_HEIGHT, PLAYER_WIDTH, PLAYER_HEIGHT)
player_dx = 0
player_dy = 0
is_jumping = False


# Game loop
running = True
while running:
    screen.fill(WHITE)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                player_dx = -PLAYER_SPEED
            elif event.key == pygame.K_RIGHT:
                player_dx = PLAYER_SPEED
            elif event.key == pygame.K_SPACE and not is_jumping:
                is_jumping = True
                player_dy = -JUMP_HEIGHT

        if event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                player_dx = 0

    # Apply gravity
    player_dy += GRAVITY

    # Update player position
    player.x += player_dx
    player.y += player_dy

    # Collision detection with ground
    if player.y >= HEIGHT - PLAYER_HEIGHT:
        player.y = HEIGHT - PLAYER_HEIGHT
        player_dy = 0
        is_jumping = False

    # Collision detection with screen boundaries
    if player.left < 0:
        player.left = 0
    elif player.right > WIDTH:
        player.right = WIDTH

    pygame.draw.rect(screen, RED, player)

    pygame.display.flip()
    pygame.time.Clock().tick(60)

pygame.quit()
sys.exit()
