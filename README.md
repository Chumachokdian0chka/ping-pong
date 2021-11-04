from pygame import *

fps = 60
game = True
finish = False
clock= time.Clock()
win_height = 500
win_width = 700
window = display.set_mode((700, 500))
display.set_caption('Ping Pong')
background = transform.scale(image.load('phone.png'), (700, 500))

speed_x = 3
speed_y = 3

font.init()
font = font.Font(None, 50)
lose_left = font.render('!PLAYER LEFT LOSE!', True, (180, 0, 0))
lose_right = font.render('!PLAYER RIGHT LOSE!', True, (180, 0, 0))

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, size_x, size_y, player_speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (size_x, size_y))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite): 
    def update_l(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_height - 80:
            self.rect.y += self.speed
    def update_r(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < win_height - 80:
            self.rect.y += self.speed


rocket_1 = Player("racket.png", 30, 10, 10, 100, 10)
rocket_2 = Player("racket.png", 650, 10, 10, 100, 10)
ball = GameSprite("tenis_ball.png", 200, 200, 50, 50, 200)

while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
    if finish != True:
        window.blit(background, (0, 0))
        rocket_1.update_l()
        rocket_2.update_r()
        ball.rect.x += speed_x
        ball.rect.y += speed_y

        if sprite.collide_rect(rocket_1, ball) or sprite.collide_rect(rocket_2, ball):
            speed_x *= -1
        if ball.rect.y > win_height-50 or ball.rect.y < 0:
            speed_y *= -1
        if ball.rect.x < 0:
            finish = True
            time.delay(3000)
            window.blit(lose_left, (200, 200))
            game_over = False
        if ball.rect.x > win_width:
            finish = True
            time.delay(3000)
            window.blit(lose_right, (200, 200))
            game = False

        rocket_1.reset()
        rocket_2.reset()
        ball.reset()
    clock.tick(fps)
    display.update()
