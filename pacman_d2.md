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
        self.img = loadImage(path+"/images/pellet.png")
        
    
    def display(self):
        image(self.img,self.x+8,self.y+8,5,5)
        

class Watermelon:                                                        #special pellets that cause sprites to be ghosts
    def __init__(self,x,y):
        self.x=x
        self.y=y
        self.status= False                                               #False to show sprites, True to show ghosts
        self.img = loadImage(path+"/images/watermelon.png")
        
    def display(self):
        image(self.img,self.x,self.y,20,20)


class Board:
    def __init__(self):
        self.blocks = []
        self.pellet = []
        self.watermelon = []
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
                elif i == '*':
                    self.watermelon.append(Watermelon(col*20,row*20))
                col+=1
            row+=1
        
    def display(self):
        for block in self.blocks:
            block.display()
        for pellet in self.pellet:
            pellet.display()
        for watermelon in self.watermelon:
            watermelon.display()
            
    def getBoard(self,x,y): 
        for b in blocks:
            col = self.x//self.dim
            row = self.y//self.dim 
            return col, row

class Creature:
    def __init__(self,x,y,v=1):
        self.x=x
        self.y=y
        self.v=v
        self.dir="RIGHT"                    
        
class Ghosts(Creature):
    def __init__(self,x,y):
        Creature.__init__(self,x,y)
        self.vx=0
        self.vy=0
        self.dim = 20 
        self.dir="RIGHT"
        self.names = ['red','orange','pink','blue']
        self.status=True                                                         # if Watermelon returns True, sprites = False  
        self.rightred = loadImage(path+'/images/redghostright1.png')
        self.leftred = loadImage(path+'/images/redspriteLEFT.png')
        self.upred = loadImage(path+'/images/redsprite.png')
        self.downred = loadImage(path+'/images/pacmandown2.png')
        
        # if Watermelon returns True, sprites = False 
        
    def update(self):
        if self.dir=='LEFT':
            self.vx = -20
            # self.vy = 0
            self.dir = 'LEFT'
            # self.imgs = [self.left1, self.left2]
        elif self.dir=='RIGHT':
            self.vx = 20
            # self.vy = 0
            self.dir = 'RIGHT'
            # self.imgs = [self.right1, self.right2]
        else:
            self.vx = 0 
        
        if self.dir == 'UP':
            # self.vx = 0
            self.vy = -20
            self.dir = 'UP'
            # self.imgs = [self.up1, self.up2]
        elif self.dir == 'DOWN':
            # self.vx = 0
            self.vy = 20
            self.dir = 'DOWN'
            # self.imgs = [self.down1, self.down2]
        else:
            self.vy = 0
        
        nextBlockSafe = self.checkNextTile()
        if nextBlockSafe == True:
            self.x += self.vx
            self.y += self.vy
        
        
    def checkNextTile(self):
        fill(255)
        
        if self.dir == 'RIGHT' or self.dir == 'LEFT':
            for b in g.board.blocks:
                if self.y == b.y and self.x+self.vx == b.x:
                    self.vx = 0
                    self.vy = 0
                    return False
            
        if self.dir == 'UP' or self.dir == 'DOWN':
            for b in g.board.blocks:
                if self.x == b.x and self.y+self.vy == b.y:
                    self.vy = 0
                    self.vx = 0
                    return False
        return True   
        
        
        
        
        
    def display(self):
        image(self.rightred,self.x,self.y,self.dim,self.dim)
        self.update()
        #self.sprite = (self.sprite + 1) % 2
        #self.update() 

        

        
class PacMan(Creature):
    def __init__(self,x,y):
        Creature.__init__(self,x,y)
        self.keyHandler = {LEFT:False, RIGHT:False, UP:False, DOWN:False}
        self.vx=0
        self.vy=0
        self.dim = 20
        self.dir = "RIGHT"
        self.sprite = 0
        self.left1 = loadImage(path+'/images/pacmanleft1.png')
        self.left2 = loadImage(path+'/images/pacmanleft2.png')
        self.right1 = loadImage(path+'/images/pacmanright1.png')
        self.right2 = loadImage(path+'/images/pacmanright2.png')
        self.up1 = loadImage(path+'/images/pacmanup1.png')
        self.up2 = loadImage(path+'/images/pacmanup2.png')
        self.down1 = loadImage(path+'/images/pacmandown1.png')
        self.down2 = loadImage(path+'/images/pacmandown2.png')
        self.imgs= [self.right1, self.right2]

    
        
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
        
        # before we add the velocities, check if PacMan is in a square that lets him move to the next block
        nextBlockSafe = self.checkNextTile()
        if nextBlockSafe == True:
            self.x += self.vx
            self.y += self.vy
        
        for p in g.board.pellet:
            if self.x == p.x and self.y == p.y:
                g.board.pellet.remove(p)
        for s in g.board.watermelon:
            if self.x == s.x and self.y == s.y:
                g.board.watermelon.remove(s)
            
    

    def display(self):
        self.sprite = (self.sprite + 1) % 2
        self.update() 

        image(self.imgs[self.sprite],self.x,self.y,self.dim,self.dim)
        #print self.vx, self.vy 
        
    # True if moveable, False if block
    def checkNextTile(self):

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
        self.ghost=Ghosts(20,20)
        self.board = Board()
        
    def display(self):
        self.board.display()
        self.pacman.display()
        self.ghost.display()
            
g = Game(540,540)

def setup():
    frameRate(7)
    size(g.w,g.h)
    fill(255, 0, 0)
    
def draw():
    background(0)
    g.display()
    

def keyPressed():
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
