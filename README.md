- 👋 Hi, I’m @aslann6525
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
aslann6525/aslann6525 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->import pygame
import time
import random

pygame.init()

# Ekran boyutları ve renkler
width, height = 800, 600
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)
green = (0, 255, 0)

# Yılan ve yiyecek başlangıç konumları ve boyutları
snake_block = 10
snake_speed = 15

# Pencere oluşturma
gameDisplay = pygame.display.set_mode((width, height))
pygame.display.set_caption('Yılan Oyunu')

# Zaman ve puan
clock = pygame.time.Clock()
font = pygame.font.SysFont(None, 35)

# Yılanın hareket yönü
direction = 'RIGHT'

# Yılanın başlangıç pozisyonu
snake_pos = [width // 2, height // 2]
snake_body = [[width // 2, height // 2], [width // 2 - 10, height // 2], [width // 2 - 20, height // 2]]

# Yiyecek başlangıç pozisyonu
food_pos = [random.randrange(1, (width//10)) * 10, random.randrange(1, (height//10)) * 10]

# Oyun döngüsü
game_over = False
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP and not direction == 'DOWN':
                direction = 'UP'
            elif event.key == pygame.K_DOWN and not direction == 'UP':
                direction = 'DOWN'
            elif event.key == pygame.K_LEFT and not direction == 'RIGHT':
                direction = 'LEFT'
            elif event.key == pygame.K_RIGHT and not direction == 'LEFT':
                direction = 'RIGHT'

    # Yılanın hareketi
    if direction == 'UP':
        snake_pos[1] -= snake_block
    elif direction == 'DOWN':
        snake_pos[1] += snake_block
    elif direction == 'LEFT':
        snake_pos[0] -= snake_block
    elif direction == 'RIGHT':
        snake_pos[0] += snake_block

    # Yılanın sınırları kontrol etme
    if snake_pos[0] >= width or snake_pos[0] < 0 or snake_pos[1] >= height or snake_pos[1] < 0:
        game_over = True

    # Yiyecek yendiğinde yılanın uzunluğunu artırma
    if snake_pos[0] == food_pos[0] and snake_pos[1] == food_pos[1]:
        food_pos = [random.randrange(1, (width//10)) * 10, random.randrange(1, (height//10)) * 10]
    else:
        snake_body.insert(0, list(snake_pos))
        if len(snake_body) > 1:
            snake_body.pop()

    # Yılanın kendisine çarpmasını kontrol etme
    for segment in snake_body[1:]:
        if segment == snake_pos:
            game_over = True

    # Ekranı temizleme
    gameDisplay.fill(black)

    # Yılan ve yiyeceği çizme
    for pos in snake_body:
        pygame.draw.rect(gameDisplay, green, [pos[0], pos[1], snake_block, snake_block])
    pygame.draw.rect(gameDisplay, red, [food_pos[0], food_pos[1], snake_block, snake_block])

    # Ekranı güncelleme
    pygame.display.update()

    # FPS ayarlama
    clock.tick(snake_speed)

# Oyun bittiğinde pygame'i kapatma
pygame.quit()

