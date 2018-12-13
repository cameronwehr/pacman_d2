import os

path=os.getcwd()

class Block:
    def __init__(self,x,y):
        self.x=x
        self.y=y
        self.img = loadImage(path+"/images/block.png")
        self.dim = 20 #dimension of a block is 20 x 20
        
    def display(self):
        image(self.img,self.x,self.y)
            
class Pellet:
    def __init__(self,x,y): 
        self.x=x
        self.y=y
        #self.status="shown" #all pellets will be shown initially
        self.img = loadImage(path+"/images/pellet.png")
        
    
    def display(self):
        image(self.img,self.x+8,self.y+8,5,5)
        # elif self.v == 'special':
        #     image(self.img,self.x,self.y)

class Board:
    def __init__(self):
        self.blocks = []
        self.pellet = []
        inputFile = open(path+"/PacMan-board.csv","r")
        row = 0
        for line in inputFile:
            line = line.strip().split(",")
            
            col = 0
            for i in line:
                if i == '#':
                    self.blocks.append(Block(col*20,row*20))
                elif i == '.':
                    self.pellet.append(Pellet(col*20,row*20))
                col+=1
            row+=1
        
        # print x and y of every block
        # for block in self.blocks:
        #     print("Block: {},{}".format(block.x,block.y))
        
    def display(self):
        for block in self.blocks:
            block.display()
        for pellet in self.pellet:
            pellet.display()
            
    def getBoard(self,x,y): 
        for b in blocks:
            col = self.x//self.dim
            row = self.y//self.dim 
            return col, row

class Creature:
    def __init__(self,x,y,v=1,vx=0,vy=0):
        self.x=x
        self.y=y
        self.v=v
        self.dir="RIGHT"
        self.left1 = loadImage(path+'/images/pacmanleft1.png')
        self.left2 = loadImage(path+'/images/pacmanleft2.png')
        self.right1 = loadImage(path+'/images/pacmanright1.png')
        self.right2 = loadImage(path+'/images/pacmanright2.png')
        self.up1 = loadImage(path+'/images/pacmanup1.png')
        self.up2 = loadImage(path+'/images/pacmanup2.png')
        self.down1 = loadImage(path+'/images/pacmandown1.png')
        self.down2 = loadImage(path+'/images/pacmandown2.png')
        self.imgs= [self.left1, self.left2]                    

class Ghosts(Creature):
    def __init__(self,x,y,v=1,vx=0,vy=0):
        self.x=x
        self.y=y
        self.v=v
        
        
class PacMan(Creature):
    def __init__(self,x,y):
        Creature.__init__(self,x,y)
        self.keyHandler = {LEFT:False, RIGHT:False, UP:False, DOWN:False}
        self.vx=0
        self.vy=0
        self.dim = 20
        self.dir = "RIGHT"
        self.sprite = 0
        
        
    def update(self):
        if self.keyHandler[LEFT]:
            self.vx = -20
            # self.vy = 0
            self.dir = 'LEFT'
            self.imgs = [self.left1, self.left2]
            # print(self.dir)
        elif self.keyHandler[RIGHT]:
            self.vx = 20
            # self.vy = 0
            self.dir = 'RIGHT'
            self.imgs = [self.right1, self.right2]
            # print(self.dir)
        else:
            self.vx = 0 
        
        if self.keyHandler[UP]:
            # self.vx = 0
            self.vy = -20
            self.dir = 'UP'
            self.imgs = [self.up1, self.up2]
            # print(self.dir)
        elif self.keyHandler[DOWN]:
            # self.vx = 0
            self.vy = 20
            self.dir = 'DOWN'
            self.imgs = [self.down1, self.down2]
            # print(self.dir)
        else:
            self.vy = 0
        
        # before we add hte velocities, check if PacMan is in a square that lets him move to the next block
        nextBlockSafe = self.checkNextTile()
        if nextBlockSafe == True:
            self.x += self.vx
            self.y += self.vy
        # print(self.x, self.y)
    
        
    # def display(self):
    #     self.update()
    #     if self.dir == 'LEFT':
    #         image(self.img)
    #     elif self.dir == 'RIGHT':
    #         image(self.img)
    #     elif self.dir == 'UP':
    #         image(self.img)
    #     elif self.dir == 'DOWN':
    #         image(self.img)
        
        # for b in bo.blocks:
        #     if self.distance(b) <= self.r + b.r:
        #         self.vx=0
        #         self.vy=0
        
    # def eatenPellets(self,
    #     if pacman passes through one pellet, it will be deleted from the pellet list 
        
    #      if self.keyHandler[LEFT] or self.keyHandler[RIGHT] :
    #         for b in g.board.pellets:
    #             if self.y == b.y and self.x+self.vx == b.x:
    #                 delete b 
    #                 return False
        

    def display(self):
        self.sprite = (self.sprite + 1) % 2
        self.update() 

        image(self.imgs[self.sprite],self.x,self.y,self.dim,self.dim)
        
        # if dir == RIGHT: 
        #     loadImage(path+'/images/pacman'+str(self.dir)+'.png')
        # elif dir == LEFT: 
        #     loadImage(path+'/images/pacman'+str(self.dir)+'.png')
        # elif dir == DOWN:
        #      loadImage(path+'/images/pacman'+str(self.dir)+'.png')
        # elif dir == UP:
        #     loadImage(path+'/images/pacman'+str(self.dir)+'.png')
        
    
        print self.vx, self.vy 
        
    # True if moveable, False if block
    def checkNextTile(self):
        # y = self.y
        # x = self.x
        # print("PacMan {},{}".format(checkR,checkC))
        
        
        
        # 1. Convert pacman's x and y coordinates to a row and column
        # print("TopLeft: {}:{}, TopRight: {}:{}, BotLeft: {}:{}, BotRight: {}:{}".format(self.x, self.y, (self.x+self.dim), self.y, self.x, (self.y+self.dim), (self.x+self.dim), (self.y+self.dim)))
        fill(255)
        
        if self.keyHandler[LEFT] or self.keyHandler[RIGHT] :
            
            for b in g.board.blocks:
                if self.y == b.y and self.x+self.vx == b.x:
                    self.vx = 0
                    self.vy = 0
                    return False
        
        if self.keyHandler[UP] or  self.keyHandler[DOWN]:
            for b in g.board.blocks:
                if self.x == b.x and self.y+self.vy == b.y:
                    self.vy = 0
                    self.vx = 0
                    return False
      
        return True   
                
        
class Game:
    def __init__(self,w,h):
        self.w=w
        self.h=h
        self.pacman=PacMan(20,20)
        self.board = Board()
        
    def display(self):
        self.board.display()
        self.pacman.display()
        
    # def getTile(self, r, c):
    #     for tile in self.tiles:
    #         if tile.r == r and tile.c == c:
    #             return tile
    #     return False   
        #print(getTile(PacMan))
        
        
g = Game(540,540)

def setup():
    frameRate(7)
    size(g.w,g.h)
    fill(255, 0, 0)
    
def draw():
    background(0)
    g.display()
    

def keyPressed():
    # print g.pacman.keyHandler
    if keyCode == LEFT:
        g.pacman.keyHandler[LEFT] = True
    elif keyCode == RIGHT:
        g.pacman.keyHandler[RIGHT] = True
    elif keyCode == UP:
        g.pacman.keyHandler[UP] = True
    elif keyCode == DOWN:
        g.pacman.keyHandler[DOWN] = True
    if key == 'x':
        g.pacman.checkNextTile()
        
def keyReleased():
    if keyCode == LEFT:
        g.pacman.keyHandler[LEFT] = False
    elif keyCode == RIGHT:
        g.pacman.keyHandler[RIGHT] = False
    elif keyCode == UP:
        g.pacman.keyHandler[UP] = False
    elif keyCode == DOWN:
        g.pacman.keyHandler[DOWN] = False
