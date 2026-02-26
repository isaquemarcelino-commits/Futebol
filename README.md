mini-football-game/
│
├── main.py
├── requirements.txt
└── README.md
import pygame
import sys

pygame.init()

# Tela
WIDTH, HEIGHT = 1000, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Mini Futebol")

# Cores
GREEN = (34, 139, 34)
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
RED = (255, 0, 0)
BLACK = (0, 0, 0)

# Relógio
clock = pygame.time.Clock()

# Jogadores
player1 = pygame.Rect(100, 250, 40, 40)
player2 = pygame.Rect(860, 250, 40, 40)

player_speed = 5

# Bola
ball = pygame.Rect(WIDTH//2 - 15, HEIGHT//2 - 15, 30, 30)
ball_speed_x = 4
ball_speed_y = 4

# Gols
goal1 = pygame.Rect(0, 200, 10, 200)
goal2 = pygame.Rect(WIDTH - 10, 200, 10, 200)

score1 = 0
score2 = 0

font = pygame.font.SysFont(None, 50)

def reset_ball():
    global ball_speed_x, ball_speed_y
    ball.center = (WIDTH//2, HEIGHT//2)
    ball_speed_x *= -1
    ball_speed_y *= -1

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()

    # Player 1 (WASD)
    if keys[pygame.K_w]: player1.y -= player_speed
    if keys[pygame.K_s]: player1.y += player_speed
    if keys[pygame.K_a]: player1.x -= player_speed
    if keys[pygame.K_d]: player1.x += player_speed

    # Player 2 (Setas)
    if keys[pygame.K_UP]: player2.y -= player_speed
    if keys[pygame.K_DOWN]: player2.y += player_speed
    if keys[pygame.K_LEFT]: player2.x -= player_speed
    if keys[pygame.K_RIGHT]: player2.x += player_speed

    # Movimento da bola
    ball.x += ball_speed_x
    ball.y += ball_speed_y

    # Colisão com bordas
    if ball.top <= 0 or ball.bottom >= HEIGHT:
        ball_speed_y *= -1

    # Colisão com jogadores
    if ball.colliderect(player1) or ball.colliderect(player2):
        ball_speed_x *= -1

    # Gol
    if ball.colliderect(goal1):
        score2 += 1
        reset_ball()

    if ball.colliderect(goal2):
        score1 += 1
        reset_ball()

    # Desenhar
    screen.fill(GREEN)

    pygame.draw.rect(screen, WHITE, goal1)
    pygame.draw.rect(screen, WHITE, goal2)
    pygame.draw.rect(screen, BLUE, player1)
    pygame.draw.rect(screen, RED, player2)
    pygame.draw.ellipse(screen, WHITE, ball)

    score_text = font.render(f"{score1} - {score2}", True, BLACK)
    screen.blit(score_text, (WIDTH//2 - 40, 20))

    pygame.display.flip()
    clock.tick(60)
