- 👋 Hi, I’m @eryk978
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
eryk978/eryk978 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import math

# Inicjalizacja pygame
pygame.init()

# Ustawienia okna gry
screen_width, screen_height = 800, 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Prosta strzelanka")

# Kolory
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Gracz
player_size = 50
player_x, player_y = screen_width // 2, screen_height // 2
player_speed = 5

# Pociski
bullet_speed = 10
bullets = []

# Funkcja do rysowania gracza
def draw_player(x, y):
    pygame.draw.rect(screen, BLACK, (x - player_size // 2, y - player_size // 2, player_size, player_size))

# Funkcja do rysowania pocisków
def draw_bullets():
    for bullet in bullets:
        pygame.draw.rect(screen, BLACK, bullet)

# Główna pętla gry
running = True
clock = pygame.time.Clock()

while running:
    screen.fill(WHITE)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Ruch gracza
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_x -= player_speed
    if keys[pygame.K_RIGHT]:
        player_x += player_speed
    if keys[pygame.K_UP]:
        player_y -= player_speed
    if keys[pygame.K_DOWN]:
        player_y += player_speed

    # Strzelanie
    if pygame.mouse.get_pressed()[0]:
        mouse_x, mouse_y = pygame.mouse.get_pos()
        angle = math.atan2(mouse_y - player_y, mouse_x - player_x)
        bullet_x = player_x + player_size // 2 * math.cos(angle)
        bullet_y = player_y + player_size // 2 * math.sin(angle)
        bullets.append({
            'rect': pygame.Rect(bullet_x, bullet_y, 10, 10),
            'angle': angle
        })

    # Aktualizacja pozycji pocisków
    for bullet in bullets[:]:
        bullet['rect'].x += bullet_speed * math.cos(bullet['angle'])
        bullet['rect'].y += bullet_speed * math.sin(bullet['angle'])
        if bullet['rect'].x < 0 or bullet['rect'].x > screen_width or bullet['rect'].y < 0 or bullet['rect'].y > screen_height:
            bullets.remove(bullet)

    # Rysowanie elementów na ekranie
    draw_player(player_x, player_y)
    draw_bullets()

    # Odświeżanie ekranu
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
