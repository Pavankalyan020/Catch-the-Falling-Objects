# Catch-the-Falling-Objects
A player-controlled block moves left and right. Red blocks fall from the top. The player scores points by catching them.

pip install pygame

import pygame
import random

# Initialize Pygame
pygame.init()

# Screen dimensions
screen_width = 800
screen_height = 600

# Colors
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)

# Create the screen
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Catch the Falling Objects")

# Clock to control the frame rate
clock = pygame.time.Clock()

# Load player image
player_img = pygame.Surface((50, 50))
player_img.fill(black)
player_rect = player_img.get_rect()
player_rect.center = (screen_width // 2, screen_height - 50)

# Falling object
object_img = pygame.Surface((30, 30))
object_img.fill(red)
object_rect = object_img.get_rect()
object_rect.x = random.randint(0, screen_width - object_rect.width)
object_rect.y = -30

# Game variables
score = 0
object_speed = 5
player_speed = 10

# Font for displaying score
font = pygame.font.SysFont(None, 36)

# Main game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Player movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_rect.left > 0:
        player_rect.x -= player_speed
    if keys[pygame.K_RIGHT] and player_rect.right < screen_width:
        player_rect.x += player_speed

    # Move the falling object
    object_rect.y += object_speed

    # Check if the object is caught
    if object_rect.colliderect(player_rect):
        score += 1
        object_rect.x = random.randint(0, screen_width - object_rect.width)
        object_rect.y = -30

    # Check if the object falls off the screen
    if object_rect.top > screen_height:
        object_rect.x = random.randint(0, screen_width - object_rect.width)
        object_rect.y = -30

    # Clear the screen
    screen.fill(white)

    # Draw player and object
    screen.blit(player_img, player_rect)
    screen.blit(object_img, object_rect)

    # Display the score
    score_text = font.render(f"Score: {score}", True, black)
    screen.blit(score_text, (10, 10))

    # Update the display
    pygame.display.flip()

    # Cap the frame rate
    clock.tick(30)

# Quit Pygame
pygame.quit()

