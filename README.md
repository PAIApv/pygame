# pygame
pip install pygame
import pygame
import random

# Inicialização do Pygame
pygame.init()

# Configurações da janela
WIDTH, HEIGHT = 800, 600
FPS = 60
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Configurações do jogador
PLAYER_WIDTH, PLAYER_HEIGHT = 10, 100
PLAYER_SPEED = 5

# Configurações da bola
BALL_SIZE = 10
BALL_SPEED_X = 4
BALL_SPEED_Y = 4

# Configurações da rede
NET_WIDTH = 2
NET_COLOR = WHITE

# Inicialização da janela do jogo
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Pong")

clock = pygame.time.Clock()

# Função para desenhar o jogador
def draw_player(x, y):
    pygame.draw.rect(screen, WHITE, pygame.Rect(x, y, PLAYER_WIDTH, PLAYER_HEIGHT))

# Função para desenhar a bola
def draw_ball(x, y):
    pygame.draw.circle(screen, WHITE, (x, y), BALL_SIZE)

# Função principal do jogo
def main():
    # Posições iniciais do jogador e da bola
    player_y = HEIGHT // 2 - PLAYER_HEIGHT // 2
    opponent_y = HEIGHT // 2 - PLAYER_HEIGHT // 2
    ball_x = WIDTH // 2
    ball_y = HEIGHT // 2
    ball_speed_x = random.choice([-BALL_SPEED_X, BALL_SPEED_X])
    ball_speed_y = random.choice([-BALL_SPEED_Y, BALL_SPEED_Y])

    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        # Controles do jogador (tecla para cima e para baixo)
        keys = pygame.key.get_pressed()
        if keys[pygame.K_UP]:
            player_y -= PLAYER_SPEED
        if keys[pygame.K_DOWN]:
            player_y += PLAYER_SPEED

        # Movimentação da bola
        ball_x += ball_speed_x
        ball_y += ball_speed_y

        # Colisão da bola com as bordas
        if ball_y - BALL_SIZE <= 0 or ball_y + BALL_SIZE >= HEIGHT:
            ball_speed_y = -ball_speed_y

        # Colisão da bola com os jogadores
        if ball_x <= PLAYER_WIDTH and player_y <= ball_y <= player_y + PLAYER_HEIGHT:
            ball_speed_x = -ball_speed_x

        if ball_x >= WIDTH - PLAYER_WIDTH and opponent_y <= ball_y <= opponent_y + PLAYER_HEIGHT:
            ball_speed_x = -ball_speed_x

        # Desenho da tela do jogo
        screen.fill(BLACK)
        draw_player(0, player_y)
        draw_player(WIDTH - PLAYER_WIDTH, opponent_y)
        pygame.draw.line(screen, NET_COLOR, (WIDTH // 2, 0), (WIDTH // 2, HEIGHT), NET_WIDTH)
        draw_ball(ball_x, ball_y)

        pygame.display.flip()
        clock.tick(FPS)

    pygame.quit()

if __name__ == "__main__":
    main()
