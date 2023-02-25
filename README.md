from pygame import *

back = (200, 255, 255)
win_width = 600
win_height = 500
window = display.set_mode((win_width, win_height))


clock = time.Clock()
FPS = 60
game = True
finish = False
class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image),( 50, 50))
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

ball = GameSprite('tenis_ball.png', 300, 250, 5)

font.init()
font1 = font.Font(None, 35)
lose1 =  font1.render('PLAYER 1 LOSE', True, (180, 0, 0))
font2 = font.Font(None, 35)
lose2 =  font1.render('PLAYER 2 LOSE', True, (180, 0, 0))

speed_x = 3
speed_y = 3

while game:
    for e in event.get():
        if e.type == QUIT:
            game = False

    if ball.rect.x < 0:
        finish =True
        window.blit(lose1, (200, 200))
    if ball.rect.x > 600:
        finish =True
        window.blit(lose2, (200, 200))

    if finish != True:
        ball.rect.x += speed_x
        ball.rect.y += speed_y

        if ball.rect.y > win_height - 50 or ball.rect.y < 0:
                speed_y *= -1
        if sprite.collide_rect(racket1, ball) or sprite.collide_rect(racket2, ball):
                speed_x *= -1            
        window.fill(back)
        racket1.reset()
        racket1.update1()

        racket2.reset()
        racket2.update2()

        ball.reset()


    display.update()
    clock.tick(FPS)
 
