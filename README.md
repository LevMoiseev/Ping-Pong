from pygame import * 

init()
WIN_WIDTH = 800
WIN_HEIGHT = 600 
window = display.set_mode((WIN_WIDTH, WIN_HEIGHT))
display.set_caption('Антон чигур никого не убивал')

BACKGROUND_COLOR = (200, 245, 255)

game = True
finish = False
clock = time.Clock()
FPS = 60

class GameSprite(sprite.Sprite):
    def __init__(self, image_path, x, y, width, height, speed):
        super().__init__()
        self.image = transform.scale(image.load(image_path), (width, height))
        self.speed = speed
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def update_l(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < WIN_HEIGHT - self.rect.height - 5:
            self.rect.y += self.speed

    def update_r(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < WIN_HEIGHT - self.rect.height - 5:
            self.rect.y += self.speed

racket1 = Player('racket.png', 30, 200, 35, 155, 10)
racket2 = Player('racket.png', WIN_WIDTH - 65, 200, 35, 155, 10)
ball = GameSprite('tenis_ball.png', WIN_WIDTH // 2, WIN_HEIGHT // 2, 3, 3, 0)

font.init()
main_font = font.Font(None, 60)
lose1 = main_font.render('PLAYER 1 LOSE!', True, (180, 0, 0))
lose2 = main_font.render('PLAYER 2 LOSE!', True, (180, 0, 0))

speed_x = 5
speed_y = 5


while game:
    for e in event.get():
        if e.type == QUIT:
            game = False

    if not finish:
        window.fill(BACKGROUND_COLOR)
        
        racket1.update_l()
        racket2.update_r()

        ball.rect.x += speed_x
        ball.rect.y += speed_y
        

        if ball.rect.y <= 0 or ball.rect.y >= WIN_HEIGHT - ball.rect.height:
            speed_y *= -1

        if sprite.collide_rect(racket1, ball) or sprite.collide_rect(racket2, ball):
            speed_x *= -1

        if ball.rect.x < 0:
            finish = True
            window.blit(lose1, (WIN_WIDTH // 2 - 150, WIN_HEIGHT // 2 - 20))

        if ball.rect.x > WIN_WIDTH - ball.rect.width:
            finish = True
            window.blit(lose2, (WIN_WIDTH // 2 - 150, WIN_HEIGHT // 2 - 20))
        racket1.reset()
        racket2.reset()
        ball.reset()

    display.update()
    clock.tick(FPS)
