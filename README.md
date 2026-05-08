class GameSprite(sprite.Sprite):
    def __init__(self, image_path, speed, x, y):
        super().__init__()
        self.image = transform.scale(image.load(image_path), (65, 65))
        self.speed = speed
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y

    def reset(self):
        win.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def update(self):
        keys_pressed = key.get_pressed()
        if keys_pressed[K_d] and self.rect.x < 595:
            self.rect.x += 10
        if keys_pressed[K_a] and self.rect.x > 5:
            self.rect.x -= 10
