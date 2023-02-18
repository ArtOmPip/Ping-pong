# Ping-pong

from pygame import *

back = (200, 255, 255)
win_width = 600
win_height = 500
window = display.set_mode((win_width, win_height))


clock = time.Clock()
FPS = 60
game = True

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image), ( 50, 50))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    
    def update2(self):
        keys = key.get_pressed() 
        if (keys[K_s]) and self.rect.y < win_height - 50:
            self.rect.y += self.speed
        if (keys[K_w]) and self.rect.y > 5:
            self.rect.y -= self.speed

    def update1(self):
        keys = key.get_pressed() 
        if (keys[K_DOWN]) and self.rect.y < win_height - 50:
            self.rect.y += self.speed
        if (keys[K_UP]) and self.rect.y > 5:
            self.rect.y -= self.speed

racket1 = Player('racket.png', 550, 300, 10)
racket2 = Player('racket.png', 0, 300, 10)

while game:
    for e in event.get():
        if e.type == QUIT:
            game = False

    window.fill(back)
    racket1.reset()
    racket1.update1()

    racket2.reset()
    racket2.update2()

    display.update()
    clock.tick(FPS)
