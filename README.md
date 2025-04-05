import pygame
import random
import time
from pygame.locals import (K_LEFT, K_RIGHT)
from pygame.color import THECOLORS
from coords import *
 
clock =pygame.time.Clock()
pygame.init()
GRAY = (169, 169, 169) 
class Coin(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load('Mario Game Assets/Mario Game Assets/Coin.png').convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.rect = self.image.get_rect(center=(x, y))
        self.start_y = y 
        self.dy = -5  
        self.gravity = 0.5  
        self.max_height = y - 20  
        self.state = "rising" 
        self.Frame = 0
    def update(self):
        if self.state == "rising":  
            self.rect.y += self.dy  
            if self.rect.y <= self.max_height:
                self.state = "floating"  
                self.dy = 0
        elif self.state == "floating":
            self.rect.y = self.max_height
        self.Frame += 0.1
        if self.Frame > 2:
            self.Frame -= 2
        Personnel = ['Coin.png', 'Coin_Underground.png']
        self.image = pygame.image.load('Mario Game Assets/Mario Game Assets/' + Personnel[int(self.Frame)]).convert_alpha()
    def remove(self):
        self.kill()
class Mushroom(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load('Mario Game Assets/Mario Game Assets/MagicMushroom.png').convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.rect = self.image.get_rect(center=(x, y))
        self.dy = -3  
        self.dx = 1 
        self.gravity = 1  
        self.blocks = blocks
    def update(self):
        self.dy += self.gravity  
        self.rect.y += self.dy  

        for block in self.blocks:
            if self.rect.colliderect(block.rect):
                self.rect.bottom = block.rect.top 
                self.dy = 0  
            if self.rect.y >height-65:
                self.rect.y =height-65
        self.rect.x += self.dx
    def remove(self):
        self.kill()
class Block(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.rect = self.image.get_rect(center=(x, y))
        self.original_y = y  
        self.is_bouncing = False 
        self.bounce_height = 0  
        self.bounce_speed = 1  
    def update(self):
        if self.is_bouncing:
            self.rect.y -= self.bounce_height 
            self.bounce_height -= self.bounce_speed  
            if self.bounce_height <= 0:
                self.is_bouncing = False
        if not self.is_bouncing and self.rect.y < self.original_y:
            self.rect.y += self.bounce_speed
    def remove(self):
        self.kill()
class Mystery_Block(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.rect = self.image.get_rect(center=(x, y))
        self.original_y = y 
        self.is_bouncing = False  
        self.bounce_height = 0  
        self.bounce_speed = 1  
        self.bonk=False
        self.has_item = False  
    def update(self):
        if self.is_bouncing:
            self.rect.y -= self.bounce_height  
            self.bounce_height -= self.bounce_speed  
            if self.bounce_height <= 0:
                self.is_bouncing = False
        if not self.is_bouncing and self.rect.y < self.original_y:
            self.rect.y += self.bounce_speed
        if self.bonk and not self.has_item:  
            self.has_item = True  
            if random.choice([True, False]):
                item = Mushroom(self.rect.centerx, self.rect.centery - 10, 16, 16)
            else:
                item = Coin(self.rect.centerx, self.rect.centery - 10, 16, 16)
            if isinstance(item, Mushroom):
                items_group.add(item)
            else:
                coins_group.add(item)

            self.image = pygame.image.load('Mario Game Assets/Mario Game Assets/EmptyBlock.png').convert_alpha() 
    def create_item(self):
        chance = random.random()  
        if chance < 0.95:  
            self.item = Coin(self.rect.centerx, self.rect.centery)
        else: 
            self.item = Mushroom(self.rect.centerx, self.rect.centery, 16, 16, 'Mario Game Assets/Mario Game Assets/Mushroom.png')
        if self.item:
            items_group.add(self.item) 
class Pipe(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.rect = self.image.get_rect(center=(x, y))
class Flag(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.rect = self.image.get_rect(center=(x, y))
class Tower(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.rect = self.image.get_rect(center=(x, y))
class Pol(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.rect = self.image.get_rect(center=(x, y))
class Line(pygame.sprite.Sprite):
    def __init__(self, x, y, width, height, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.image = pygame.transform.scale(self.image, (width, height))
        self.image.fill((255, 255, 255), special_flags=pygame.BLEND_RGBA_SUB)
        self.rect = self.image.get_rect(center=(x, y))
class Enemy(pygame.sprite.Sprite):
    def __init__(self, x, y, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.w = 40
        self.h = 40
        self.image = pygame.transform.scale(self.image, (self.w, self.h))
        self.rect = self.image.get_rect(center=(x, y))
        self.dx = 0
        self.dy = 0
        self.onGround = False
        self.Go = True
        self.Frame = 0
        self.Left = False
        self.Right = True
        self.Stand = True
        self.trigger = False
        self.speed = 1
        self.direction = 1  
        self.live = True
        self.last_move_time = None
        self.dead_time = 0
        self.timer_start = None
        self.timer_duration = 200
        self.timer_running = False
        self.font = pygame.font.Font(None, 36)
        self.collipol = True

    def update(self, pipes, blocks, rlocks):
        self.rect.y += self.dy
        if self.collipol == False:
            self.dy += 1

        if self.live:
            self.rect.x += self.speed * self.direction
            self.rect.x += self.dx
            self.onGround = False

            if self.rect.y > height - 65 and self.collipol:
                self.rect.y = height - 65
                self.onGround = True

            self.Frame += 0.1
            if self.Frame > 2:
                self.Frame -= 2

            Personnel = ['Goomba_Walk1.png', 'Goomba_Walk2.png']
            self.image = pygame.image.load('mwalk/' + Personnel[int(self.Frame)]).convert_alpha()
            for pipe in pipes:
                if pygame.sprite.collide_rect(self, pipe):
                    self.direction *= -1
            for block in blocks:
                if pygame.sprite.collide_rect(self, block):
                    self.direction *= -1  
            for rlock in rlocks:
                if pygame.sprite.collide_rect(self, rlock):
                    self.direction *= -1  
        else:
            self.dead_time = time.time()
            self.image = pygame.image.load('Mario Game Assets/Mario Game Assets/Goomba_Flat.png').convert_alpha()
            if self.timer_running == False:
                self.timer_start = pygame.time.get_ticks()
                self.timer_running = True
            if self.timer_running:
                self.elapsed_time = pygame.time.get_ticks() - self.timer_start
                self.remaining_time = max(0, self.timer_duration - self.elapsed_time)
                if self.remaining_time == 0:
                    self.remove()

    def remove(self):
        self.kill()
class Wall(pygame.sprite.Sprite):
    def __init__(self, color, width, height, x, y):
        super().__init__()
        self.image = pygame.Surface((width, height))
        self.image=pygame.image.load("linev.png")
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.speed = 0
class Me(pygame.sprite.Sprite):
    def __init__(self, x, y, file):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.image.load(file).convert_alpha()
        self.rect = self.image.get_rect(center=(x, y))
        self.dx = 0
        self.dy = 0
        self.onGround = False
        self.Go = False
        self.Frame = 0
        self.Left = False
        self.Right = True
        self.Jump = False
        self.Stand = True
        self.Sit = False
        self.Beats = False
        self.ouch = False
        self.live = True
        self.speed = 0
        self.direction = 4
        self.run = False
        self.jumped_on_enemy = False
        self.numa = False  
        self.large = False 
        self.last_move_time = None
        self.dead_time = 0 
        self.timer_start = None
        self.timer_duration = 10000 
        self.timer_running = False
        self.font = pygame.font.Font(None, 36)
        self.immortality = False  
        self.he = False
        self.elapsed_time = None
        self.victory = False
        self.on_flag = False 
        self.flag_descend_speed = 0.2 
        self.is_descending = False  
        self.descend_acceleration = 0.05  
        self.collipol = True
        self.just_got_mushroom = False 
        self.state = "normal" 
        self.run_to_castle = False  
        self.run_speed = 2 
        self.ga = False  
        self.touched_ground_after_flag = False  

    def update(self):
        if self.state == "normal":
            self.rect.x += self.speed
            if not self.onGround:
                self.dy += 1
            self.rect.y += self.dy
            self.onGround = False
            if self.collipol == False:
                self.onGround = False
                self.dy += 1
            if self.large:
                if self.rect.y > height - 80 and self.collipol:
                    self.rect.y = height - 80
                    self.dy = 1
                    self.onGround = True
                    self.Jump = False
                else:
                    self.onGround = False
                    self.Jump = True
            else:
                if self.rect.y > height - 65 and self.live and self.collipol:
                    self.rect.y = height - 65
                    self.dy = 1
                    self.onGround = True
                    self.Jump = False
                else:
                    self.onGround = False
                    self.Jump = True
            if self.ouch:
                self.large = False
                if self.immortality:
                    self.dead_time = time.time() 
                    if self.timer_running == False:
                        self.timer_start = pygame.time.get_ticks() 
                        self.timer_running = True
                    if self.timer_running:
                        self.elapsed_time = pygame.time.get_ticks() - self.timer_start
                        self.remaining_time = max(0, self.timer_duration - self.elapsed_time) 
                        if self.remaining_time == 0:
                            self.immortality = False
            self.animate() 
        elif self.state == "on_flag":
            self.descend_from_flag()
        elif self.state == "run_to_castle":
            self.run_to_castle_animation()

    def descend_from_flag(self):
        if self.rect.y < height - 65:  
            self.rect.y += self.flag_descend_speed  
            self.flag_descend_speed += 0.005 
            if self.flag_descend_speed > 1:  
                self.flag_descend_speed = 1
            file_prefix = 'mwalk/right1' if self.Right else 'mwalk/left1'
            self.Frame += 0.1
            if self.Frame > 2:
                self.Frame -= 2
            Personnel = ['1.png', '2.png']
            self.image = pygame.image.load(f'{file_prefix}/{Personnel[int(self.Frame)]}').convert_alpha()
        else:
            self.state = "run_to_castle" 
            self.run_to_castle = True
            self.Go = True
            self.touched_ground_after_flag = True  
            file_prefix = 'mwalk/right' if self.Right else 'mwalk/left'
            if self.Go:
                self.Frame += 0.2
                if self.Frame > 5:
                    self.Frame -= 5
                Personnel = ['1.png', '2.png', '3.png', '4.png', '5.png']
                self.image = pygame.image.load(f'{file_prefix}/{Personnel[int(self.Frame)]}').convert_alpha()

    def run_to_castle_animation(self):
        if self.rect.x < tower.rect.x: 
            self.speed = self.run_speed 
            self.rect.x += self.speed
            self.Go = True
            self.Left = False
            self.Right = True
            self.Stand = False
            self.animate() 
        else:
            self.remove()  

    def animate(self):
        if self.large:
            file_prefix = 'bwalk/right' if self.Right else 'bwalk/left'
        else:
            file_prefix = 'mwalk/right' if self.Right else 'mwalk/left'
        if not self.Jump:
            if self.Go:
                self.Frame += 0.2
                if self.Frame > 5:
                    self.Frame -= 5
                Personnel = ['1.png', '2.png', '3.png', '4.png', '5.png']
                self.image = pygame.image.load(f'{file_prefix}/{Personnel[int(self.Frame)]}').convert_alpha()
            else:
                self.image = pygame.image.load(f'{file_prefix}/1.png').convert_alpha()
width, height = 1000, 250
c_x = 0
c_y = 0
c_x = 0
c_y = 0
f_x = 3600
f_y = 100
center_x=300
center_y=500
c_xx = 0
c_yy = 212
c2_xx = 0
c2_yy = 0
t_x = 3700
t_y = 157
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Super Mario Online (Base)")
lol = True
plac=0
coll = False
bgend = False
screen.fill(THECOLORS['blue'])
font = pygame.font.SysFont('couriernew', 40)
font2 = pygame.font.SysFont('couriernew', 30)
pipes = pygame.sprite.Group()
enemies = pygame.sprite.Group()
blocks = pygame.sprite.Group()
rlocks = pygame.sprite.Group()
mystery_blocks = pygame.sprite.Group()
items_group = pygame.sprite.Group()
coins_group = pygame.sprite.Group()
clock = pygame.time.Clock()
block0 = Mystery_Block(b_x0, b_y0, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block1 = Block(b_x1, b_y1, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block2 = Mystery_Block(b_x2, b_y2, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block3 = Block(b_x3, b_y3, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block4 = Mystery_Block(b_x4, b_y4, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block5 = Block(b_x5, b_y5, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block6 = Block(b_x6, b_y6, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block7 = Mystery_Block(b_x7, b_y7, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block8 = Block(b_x8, b_y8, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block9 = Block(b_x9, b_y9, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block10 = Block(b_x10, b_y10, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block11 = Block(b_x11, b_y11, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block12 = Mystery_Block(b_x12, b_y12, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png')  
block13 = Mystery_Block(b_x13, b_y13, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block14 = Mystery_Block(b_x14, b_y14, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block15 = Block(b_x15, b_y15, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block16 = Block(b_x16, b_y16, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block17 = Block(b_x17, b_y17, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')   
block18 = Block(b_x18, b_y18, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block19 = Block(b_x19, b_y19, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block20 = Mystery_Block(b_x20, b_y20, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block21 = Block(b_x21, b_y21, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block22 = Mystery_Block(b_x22, b_y22, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block23 = Block(b_x23, b_y23, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block24 = Block(b_x24, b_y24, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')   
block25 = Block(b_x25, b_y18, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block26 = Block(b_x26, b_y26, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')
block27 = Block(b_x27, b_y27, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block28 = Block(b_x28, b_y28, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block29 = Block(b_x29, b_y29, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')   
block30 = Block(b_x30, b_y30, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block31 = Block(b_x31, b_y31, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block32 = Block(b_x32, b_y32, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')   
block33 = Block(b_x33, b_y33, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block34 = Mystery_Block(b_x34, b_y34, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png')
block35 = Mystery_Block(b_x35, b_y35, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block36 = Block(b_x36, b_y36, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block37 = Block(b_x37, b_y37, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block38 = Block(b_x38, b_y38, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png')  
block39 = Mystery_Block(b_x39, b_y39, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png') 
block40 = Mystery_Block(b_x40, b_y40, 17, 16, 'Mario Game Assets/Mario Game Assets/MysteryBlock.png')  
block41 = Block(b_x41, b_y41, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block42 = Block(b_x36, b_y36, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
block43 = Block(b_x37, b_y37, 16, 16, 'Mario Game Assets/Mario Game Assets/Brick.png') 
rlock0 = Block(br_x0, br_y0, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock1 = Block(br_x1, br_y1, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock2 = Block(br_x2, br_y2, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock3 = Block(br_x3, br_y3, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock4 = Block(br_x4, br_y4, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock5 = Block(br_x5, br_y5, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock6 = Block(br_x6, br_y6, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock7 = Block(br_x7, br_y7, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock8 = Block(br_x8, br_y8, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock9 = Block(br_x9, br_y9, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock10 = Block(br_x10, br_y10, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock11 = Block(br_x11, br_y11, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock12 = Block(br_x12, br_y12, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock13 = Block(br_x13, br_y13, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock14 = Block(br_x14, br_y14, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock15 = Block(br_x15, br_y15, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock16 = Block(br_x16, br_y16, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock17 = Block(br_x17, br_y17, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')   
rlock18 = Block(br_x18, br_y18, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock19 = Block(br_x19, br_y19, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock20 = Block(br_x20, br_y20, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock21 = Block(br_x21, br_y21, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock22 = Block(br_x22, br_y22, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock23 = Block(br_x23, br_y23, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock24 = Block(br_x24, br_y24, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')   
rlock25 = Block(br_x25, br_y18, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock26 = Block(br_x26, br_y26, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')
rlock27 = Block(br_x27, br_y27, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock28 = Block(br_x28, br_y28, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock29 = Block(br_x29, br_y29, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')   
rlock30 = Block(br_x30, br_y30, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock31 = Block(br_x31, br_y31, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock32 = Block(br_x32, br_y32, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')   
rlock33 = Block(br_x33, br_y33, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock34 = Block(br_x34, br_y34, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')
rlock35 = Block(br_x35, br_y35, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock36 = Block(br_x36, br_y36, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock37 = Block(br_x37, br_y37, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock38 = Block(br_x38, br_y38, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock39 = Block(br_x39, br_y39, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock40 = Block(br_x40, br_y40, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock41 = Block(br_x41, br_y41, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock42 = Block(br_x36, br_y36, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock43 = Block(br_x37, br_y37, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')
rlock44 = Block(br_x37, br_y37, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')
rlock45 = Block(br_x38, br_y38, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock46 = Block(br_x39, br_y39, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock47 = Block(br_x40, br_y40, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock48 = Block(br_x41, br_y41, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock49 = Block(br_x42, br_y42, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock50 = Block(br_x43, br_y43, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock51 = Block(br_x44, br_y44, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock52 = Block(br_x45, br_y45, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock53 = Block(br_x46, br_y46, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock54 = Block(br_x47, br_y47, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock55 = Block(br_x48, br_y48, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock56 = Block(br_x49, br_y49, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock57 = Block(br_x50, br_y50, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock58 = Block(br_x51, br_y51, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock59 = Block(br_x52, br_y52, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock60 = Block(br_x53, br_y53, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock61 = Block(br_x54, br_y54, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')   
rlock62 = Block(br_x55, br_y55, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock63 = Block(br_x56, br_y56, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock64 = Block(br_x57, br_y57, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock65 = Block(br_x58, br_y58, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock66 = Block(br_x59, br_y59, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock67 = Block(br_x60, br_y60, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock68 = Block(br_x61, br_y61, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')   
rlock69 = Block(br_x62, br_y62, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock70 = Block(br_x63, br_y63, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')
rlock71 = Block(br_x64, br_y64, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock72 = Block(br_x65, br_y65, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock73 = Block(br_x66, br_y66, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')   
rlock74 = Block(br_x67, br_y67, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock75 = Block(br_x68, br_y68, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock76 = Block(br_x69, br_y69, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')   
rlock77 = Block(br_x70, br_y70, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock78 = Block(br_x71, br_y71, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')
rlock79 = Block(br_x72, br_y72, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock80 = Block(br_x73, br_y73, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock81 = Block(br_x74, br_y74, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock82 = Block(br_x75, br_y75, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock83 = Block(br_x76, br_y76, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock84 = Block(br_x77, br_y77, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')  
rlock85 = Block(br_x78, br_y78, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock86 = Block(br_x79, br_y79, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png') 
rlock87 = Block(br_x80, br_y80, 16, 16, 'Mario Game Assets/Mario Game Assets/HardBlock.png')
rlocks.add(rlock0, rlock1, rlock2, rlock3, rlock4, rlock5, rlock6, rlock7, rlock8, rlock9, rlock10, rlock11, rlock12, rlock13, rlock14, rlock15, rlock16, rlock17, rlock18, rlock19, rlock20, rlock21, rlock22, rlock23, rlock24, rlock25, rlock26, rlock27, rlock28, rlock29, rlock30, rlock31, rlock32, rlock33, rlock34, rlock35, rlock36, rlock37, rlock38, rlock39, rlock40, rlock41, rlock42, rlock43, rlock44, rlock45, rlock46, rlock47, rlock48, rlock49, rlock50, rlock51, rlock52, rlock53, rlock54, rlock55, rlock56, rlock57, rlock58, rlock59, rlock6, rlock61, rlock62, rlock63, rlock64, rlock65, rlock66, rlock67, rlock68, rlock69, rlock70, rlock71, rlock72, rlock73, rlock74, rlock75, rlock76, rlock77, rlock78, rlock79, rlock80, rlock81, rlock82, rlock83, rlock84, rlock85, rlock86, rlock87)
blocks.add(block1, block3, block5, block6, block8, block9, block10, block11, block15, block16, block17, block18, block19, block20, block21, block23, block24, block25, block26, block27, block28, block29, block30, block31, block32, block33, block36, block37, block38, block41)
pipe0 = Pipe(c_x0, c_y0, 35, 32, 'Mario Game Assets/Mario Game Assets/PipeSmall.png')  
pipe1 = Pipe(c_x1, c_y1, 35, 45, 'Mario Game Assets/Mario Game Assets/PipeAvarge.png')  
pipe2 = Pipe(c_x2, c_y2, 35, 63, 'Mario Game Assets/Mario Game Assets/PipeBig.png') 
pipe3 = Pipe(c_x3, c_y3, 35, 63, 'Mario Game Assets/Mario Game Assets/PipeBig.png') 
pipe4 = Pipe(c_x4, c_y4, 35, 32, 'Mario Game Assets/Mario Game Assets/PipeSmall.png') 
pipe5 = Pipe(c_x5, c_y5, 35, 32, 'Mario Game Assets/Mario Game Assets/PipeSmall.png') 
font = pygame.font.Font(None, 20)
laa = 0
laal = 0
seconds=40000
text = font.render("Wold 1-1", True, (255, 255, 255))
place = text.get_rect(center=(250, 20))
text2 = font.render(f"coins {laa}", True, (255, 255, 255))
place2 = text2.get_rect(center=(170, 20))
text3 = font.render(f"score {laal}", True, (255, 255, 255))
place3 = text3.get_rect(center=(90, 20))
text4 = font.render(f"time {seconds}", True, (255, 255, 255))
place4 = text4.get_rect(center=(350, 20))
text5 = font.render(f"level", True, (255, 255, 255))
place5 = text5.get_rect(center=(450, 20))
pipes = pygame.sprite.Group()  
enemies = pygame.sprite.Group()
pipes.add(pipe0, pipe1, pipe2, pipe3, pipe4, pipe5)
mystery_blocks.add (block0, block2, block4, block7, block12, block13, block14, block20, block22, block34, block35, block39, block40)
me = Me (center_x, center_y, 'mwalk/right/1.png')
sound = pygame.mixer.Sound("mario audio/mario_ost.mp3")
destroy_sound = pygame.mixer.Sound("mario audio/smb_breakblock.wav")
win_sound = pygame.mixer.Sound("mario audio/winner.mp3")
pygame.mixer.music.load("mario audio/mario_ost.mp3")
if me.ga==False:
    sound.play()
else:
    sound.stop()
destroy_sound.set_volume(1.0)
sound.set_volume(1.0)
def spawn_enemies(num_enemies, min_x, max_x, y, pipes):
    enemies = pygame.sprite.Group()
    for _ in range(num_enemies):
        valid_spawn = False
        while not valid_spawn:
            x = random.randint(min_x, max_x)  
            new_enemy = Enemy(x, y, 'mwalk/Goomba_Walk1.png')
            valid_spawn = True
            for pipe in pipes:
                if new_enemy.rect.colliderect(pipe.rect): 
                    valid_spawn = False
                    break  
            
        enemies.add(new_enemy)  
    return enemies
gran = pygame.Rect(c_x, c_y, 10, 200)
flags_group = pygame.sprite.Group()
flag = Flag(f_x, f_y, 18, 180, 'Mario Game Assets/Mario Game Assets/FlagPole.png')
flags_group.add(flag) 
bg_up = pygame.image.load("bg-1-1-a-небо.png")
bg_pol_group = pygame.sprite.Group()  
tower = Tower(t_x, t_y, 85, 85, "Mario Game Assets/Mario Game Assets/Castle.png")
bg_pol1 = Pol(bg_x0, bg_y0, 1050, 25, "bg-1-1-a-полы0.png")
bg_pol2 = Pol(bg_x1, bg_y1, 223, 25, "bg-1-1-a-полы1.png")
bg_pol3 = Pol(bg_x2, bg_y2, 1013, 25, "bg-1-1-a-полы2.png")
bg_pol4 = Pol(bg_x3, bg_y3, 930, 25, "bg-1-1-a-полы3.png")
bg_pol5 = Pol(bg_x4, bg_y4, 930, 25, "bg-1-1-a-полы3.png")
bg_pol_group.add(bg_pol1, bg_pol2, bg_pol3, bg_pol4, bg_pol5)  
line_group= pygame.sprite.Group()  
towers= pygame.sprite.Group()
line_pol1 = Line(l_x0, l_y0, 20, 5, "line.png")
line_pol2 = Line(l_x1, l_y1, 39, 5, "line.png")
line_pol3 = Line(l_x2, l_y2, 20, 5, "line.png")
line_group.add(line_pol1, line_pol2, line_pol3)  
towers.add(tower)
clock = pygame.time.Clock()
RED = (255, 0, 0)
while lol:
    seconds -= 1                                            
    text4 = font.render(f"time {seconds}", True, (255, 255, 255))
    press = pygame.key.get_pressed()
    pygame.display.update()
    keys=pygame.key.get_pressed()
    if me.touched_ground_after_flag and me.onGround:
        me.touched_ground_after_flag = False
        me.image = pygame.image.load('mwalk/right/1.png').convert_alpha()
    if len(enemies) < 25:
        new_enemies = spawn_enemies(5, 400, width+1700, height - 45, pipes)
        enemies.add(*new_enemies)
    for pipe in pipes:
        for enemy in enemies:
            if pygame.sprite.collide_rect(enemy, pipe):
                enemy.direction *= -1
    for pipe in pipes:
        if pygame.sprite.collide_rect(me, pipe):
            if me.Jump == False:
                if me.rect.centerx <= pipe.rect.centerx:
                    me.rect.centerx=pipe.rect.centerx-25
                elif me.rect.centerx >= pipe.rect.centerx:
                    me.rect.centerx=pipe.rect.centerx+25
            if me.rect.bottom > pipe.rect.top and me.Jump == True:
                me.dy=0
                me.rect.bottom = pipe.rect.top 
                me.onGround = True
                me.Jump == False
            if me.rect.bottomleft >= pipe.rect.topright:
                me.dy=0
                me.rect.bottomleft = pipe.rect.topright 
                me.onGround = True
                me.Jump == False
            elif me.rect.bottomright <= pipe.rect.topleft:
                me.dy=0
                me.rect.bottomright = pipe.rect.topleft
                me.onGround = True
                me.Jump == False 
    for block in blocks:
        if pygame.sprite.collide_rect(me, block):
            if me.dy > 0 and me.rect.bottom > block.rect.top:
                me.rect.bottom = block.rect.top
                me.dy = 0
                me.onGround = True
                me.Jump = False
            elif me.dy < 0 and me.rect.top < block.rect.bottom and me.rect.top - me.dy >= block.rect.bottom:
                me.rect.top = block.rect.bottom
                me.dy = 0
                block.bonk = True
                if not block.is_bouncing:
                    block.is_bouncing = True
                    block.bounce_height = 3
                if me.large: 
                    destroy_sound.play()
                    block.remove()
                    laal += 50                                        
                    text3 = font.render(f"score {laal}", True, (255, 255, 255))
            elif me.rect.right > block.rect.left and me.rect.left < block.rect.right:
                if me.rect.centerx < block.rect.centerx:
                    me.rect.right = block.rect.left
                else:
                    me.rect.left = block.rect.right
    for rlock in rlocks:
        if pygame.sprite.collide_rect(me, rlock):
            if me.dy > 0 and me.rect.bottom > rlock.rect.top:
                me.rect.bottom = rlock.rect.top
                me.dy = 0
                me.onGround = True
                me.Jump = False
            elif me.dy < 0 and me.rect.top < rlock.rect.bottom and me.rect.top - me.dy >= rlock.rect.bottom:
                me.rect.top = rlock.rect.bottom
                me.dy = 0
            elif me.rect.right > rlock.rect.left and me.rect.left < rlock.rect.right:
                if me.rect.centerx < rlock.rect.centerx:
                    me.rect.right = rlock.rect.left
                else:
                    me.rect.left = rlock.rect.right
    if me.large:
        for block in blocks:
            if pygame.sprite.collide_rect(me, block):
                if me.rect.bottom >= block.rect.top and me.rect.top < block.rect.bottom-40:
                    if me.dy > 0: 
                        me.rect.bottom = block.rect.top-40  
                        me.dy = 0
                        me.onGround = True
                        me.Jump = False
    for block in mystery_blocks:
        if pygame.sprite.collide_rect(me, block):
            if me.dy > 0 and me.rect.bottom > block.rect.top:
                me.rect.bottom = block.rect.top
                me.dy = 0
                me.onGround = True
                me.Jump = False
            elif me.dy < 0 and me.rect.top < block.rect.bottom and me.rect.top - me.dy >= block.rect.bottom:
                me.rect.top = block.rect.bottom
                me.dy = 0
                block.bonk = True
                if not block.is_bouncing:
                    block.is_bouncing = True
                    block.bounce_height = 3
            elif me.rect.right > block.rect.left and me.rect.left < block.rect.right:
                if me.rect.centerx < block.rect.centerx:
                    me.rect.right = block.rect.left
                else:
                    me.rect.left = block.rect.right
    for item in items_group: 
        if pygame.sprite.collide_rect(me, item): 
            item.remove()  
            laal += 1000                                         
            text3 = font.render(f"score {laal}", True, (255, 255, 255))
            me.large=True
    for coin in coins_group: 
        if pygame.sprite.collide_rect(me, coin): 
            coin.remove()  
            laa += 1
            text2 = font.render(f'coins {laa}', True, (255, 255, 255))
            laal += 200                                          
            text3 = font.render(f"score {laal}", True, (255, 255, 255))
    for line in line_group: 
        if pygame.sprite.collide_rect(me, line): 
            me.collipol=False
        if me.large:
            line.rect.y =+182
    for enemy in enemies:
        if pygame.sprite.collide_rect(me, enemy):
            if me.dy > 0 and me.rect.bottom > enemy.rect.top and me.rect.bottom - me.dy <= enemy.rect.top and me.Jump and me.live: 
                enemy.live = False
                me.jumped_on_enemy = True
                me.dy=-9
                laal += 100                                          
                text3 = font.render(f"score {laal}", True, (255, 255, 255))
            elif pygame.sprite.collide_rect(me, enemy) and me.rect.x >= enemy.rect.x and enemy.live == True:
                if me.large and not me.just_got_mushroom:  
                    me.large = False
                    me.immortality = True
                    enemy.live=False
                elif not me.large:  
                    me.live = False
                    me.jumped_on_enemy = False
        elif enemy.rect.x < 0:
            enemy.direction *= -1
        for pipe in pipes: 
            if pygame.sprite.collide_rect(enemy, pipe): 
                enemy.direction *= -1
        for line in line_group: 
            if pygame.sprite.collide_rect(enemy, line): 
                enemy.collipol=False
                enemy.onGround =True
        if me.just_got_mushroom:
            me.just_got_mushroom = False  
        for rlock in rlocks:
            if pygame.sprite.collide_rect(enemy, rlock):
                if enemy.rect.right > rlock.rect.left and enemy.rect.left < rlock.rect.right:
                    if enemy.rect.centerx < rlock.rect.centerx:
                        enemy.rect.right = rlock.rect.left
                        enemy.direction *= -1
                    else:
                        enemy.rect.left = rlock.rect.right
                        enemy.direction *= -1
    if pygame.sprite.collide_rect_ratio(gran):
        me.speed = 0    
    if pygame.sprite.collide_rect(me, flag):  
        me.state = "on_flag" 
        me.ga=True
        win_sound.play()
        
    if me.state == "on_flag" and me.rect.y >= height - 65:
        me.state = "run_to_castle"
        me.gag=True  
    if pygame.sprite.collide_rect(me, tower):
        me.remove()  
        victory_text = font.render("You Win!", True, (255, 255, 255))
        screen.blit(victory_text, (width // 2 - 50, height // 2))
        pygame.display.update()
        pygame.time.wait(3000)  
        lol = False  
    if me.live: 
        if keys[pygame.K_d] and coll ==False:
            me.speed = 20
            me.Go = True
            me.Left = False
            me.Right = True
            me.Stand = False
            if me.rect.x > 450:
                me.rect.x = 450
                c_x -= me.speed + 1.5
                f_x -= me.speed + 1.2 
                c2_xx -= me.speed + 1.5
                for pipe in pipes:
                    pipe.rect.x -= me.speed + 1.5
                for enemy in enemies:
                    enemy.rect.x -= me.speed + 1.5
                for block in blocks:
                    block.rect.x -= me.speed + 1.5
                for block in mystery_blocks:
                    block.rect.x -= me.speed + 1.5
                for rlock in rlocks:
                    rlock.rect.x -= me.speed + 1.5
                for bg in bg_pol_group:
                    bg.rect.x -= me.speed + 1.2
                for line in line_group:
                    line.rect.x -= me.speed + 1.2
                for coin in coins_group:
                    coin.rect.x -= me.speed + 1.2
                for mushroom in items_group:
                    mushroom.rect.x -= me.speed + 1.2
                flag.rect.x = f_x
                for tower in towers:
                    tower.rect.x -= me.speed + 1.2
        elif keys[pygame.K_a] and me.rect.x>=0 and coll ==False:
            me.speed = -2
            me.Go = True
            me.Left = True
            me.Right = False
            me.Stand = False
            if me.rect.x <0:
                me.rect.x=0
        if keys[pygame.K_SPACE]:
            if me.onGround:
                me.dy = -14
                me.onGround = False
                me.Jump = True
                me.Stand = False
        if keys[pygame.K_g]:
            me.he = True
    else:
        if me.onGround:
                me.live=False
                me.dy = -14
                me.onGround = False
                me.Jump = True
                me.Stand = False
                me.image = pygame.image.load('mwalk/Mario_Small_Death.png').convert_alpha()
    clock.tick(60)
    screen.fill((141, 201, 195))
    screen.blit(bg_up, (c_xx2, c_yy2))
    bg_pol_group.draw(screen)  
    pygame.draw.rect(screen, RED, gran)
    flags_group.draw(screen)  
    blocks.draw(screen)
    rlocks.draw(screen)
    mystery_blocks.draw(screen)
    line_group.draw(screen)  
    mystery_blocks.update()
    blocks.update()
    rlocks.update()
    pipes.draw(screen)
    enemies.draw(screen)
    items_group.draw(screen)
    items_group.update()
    coins_group.draw(screen)
    coins_group.update()
    screen.blit(text, place)
    screen.blit(text2, place2)
    screen.blit(text3, place3)
    screen.blit(text4, place4)
    me.update()
    towers.draw(screen)  
    screen.blit(me.image, me.rect)
    enemies.update(pipes, blocks, rlocks)
    if c_x < width-800 and c_xx < width-800:
        bgend = False
    else:
        bgend = True
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            lol = False
            pygame.quit()
        elif event.type == pygame.KEYUP:
            me.speed=0
            me.Go = False
            me.Stand = True
