import os

path=os.getcwd()

class Block:
    def __init__(self,x,y):
        self.x=x
        self.y=y
        self.img = loadImage(path+"/images/block.png")
        self.dim = 20 #dimension of a block is 20 x 20
        
    def display(self):
        image(self.img,self.x*self.dim,self.y*self.dim)
            
class Pellet:
    def __init__(self,x,y): 
        self.x=x
        self.y=y
        self.img = loadImage(path+"/images/pellet.png")
        
    
    def display(self):
        image(self.img,self.x*20+8,self.y*20+8,5,5)
        # elif self.v == 'special':
        #     image(self.img,self.x,self.y)

class Board:
    def __init__(self):
        self.blocks = []
        self.pellet = []
        inputFile = open(path+"/PacMan-board.csv","r")
        y = 0
        for line in inputFile:
            line = line.strip().split(",")
            
            x = 0
            for i in line:
                if i == '#':
                    self.blocks.append(Block(x,y))
                elif i == '.':
                    self.pellet.append(Pellet(x,y))
                x+=1
            y+=1
        
        # print x and y of every block
        for block in self.blocks:
            print("Block: {},{}".format(block.x,block.y))
        
    def display(self):
        for block in self.blocks:
            block.display()
        for pellet in self.pellet:
            pellet.display()

class Creature:
    def __init__(self,x,y,v=1,vx=0,vy=0):
        self.x=x
        self.y=y
        self.v=v
        self.dir="RIGHT"
        self.img=loadImage(path+'/images/pacman'+str(self.dir)+'.png')
    
        
class PacMan(Creature):
    def __init__(self,x,y):
        Creature.__init__(self,x,y)
        self.keyHandler = {LEFT:False, RIGHT:False, UP:False, DOWN:False}
        self.vx=0
        self.vy=0
        self.dim = 18
        self.dir = "RIGHT"
        
        
    def update(self):
        if self.keyHandler[LEFT]:
            self.vx = -1
            self.vy = 0
            # self.dir = 'LEFT'
            print(self.dir)
        elif self.keyHandler[RIGHT]:
            self.vx = 1
            self.vy = 0
            # self.dir = 'RIGHT'
            print(self.dir)
        elif self.keyHandler[UP]:
            self.vx = 0
            self.vy = -1
            # self.dir = 'UP'
            print(self.dir)
        elif self.keyHandler[DOWN]:
            self.vx = 0
            self.vy = 1
            # self.dir = 'DOWN'
            print(self.dir)
        else:
            self.vx = 0
            self.vy = 0
        
        # before we add hte velocities, check if PacMan is in a square that lets him move to the next block
        nextBlockSafe = self.checkNextTile()
        if nextBlockSafe == True:
            self.x += self.vx
            self.y += self.vy
        
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
        
        
        

    def display(self):
        image(self.img,self.x,self.y,self.dim,self.dim)
        self.update()
        
    # True if moveable, False if block
    def checkNextTile(self):
        checkC = self.y//self.dim
        checkR = self.x//self.dim
        print("PacMan {},{}".format(checkR,checkC))
        # 1. Convert pacman's x and y coordinates to a row and column
        # print("TopLeft: {}:{}, TopRight: {}:{}, BotLeft: {}:{}, BotRight: {}:{}".format(self.x, self.y, (self.x+self.dim), self.y, self.x, (self.y+self.dim), (self.x+self.dim), (self.y+self.dim)))
        if self.dir == 'LEFT':
        #     print("CAST FROM TOPLEFT AND BOTTOMLEFT IN LEFT DIRECTION")
        # elif self.dir == 'RIGHT':
        #     print("CAST FROM TOPRIGHT AND BOTRIGHT IN RIGHT DIRECTION")
        # elif self.dir == 'UP':
        #     print("CAST FROP TOPLEFT AND TOPRIGHT IN UP DIRECTION")
        # elif self.dir == 'DOWN':
        #     print("CAST FROP BOTLEFT AND BOTRIGHT IN DOWN DIRECTION")
        # print("IF BOTH CASTS GAVE EMPTY, WE CAN MOVE IN THAT DIRECTION")
        else:
            return False
        
    


        
class Game:
    def __init__(self,w,h):
        self.w=w
        self.h=h
        self.pacman=PacMan(20,20)
        self.board = Board()
        
    def display(self):
        self.pacman.display()
        self.board.display()
        
    # def getTile(self, r, c):
    #     for tile in self.tiles:
    #         if tile.r == r and tile.c == c:
    #             return tile
    #     return False   
        #print(getTile(PacMan))
        
        
g = Game(540,540)

def setup():
    size(g.w,g.h)
    
def draw():
    background(0)
    g.display()


def keyPressed():
    if keyCode == LEFT:
        g.pacman.keyHandler[LEFT] = True
        g.pacman.dir = "LEFT"
    elif keyCode == RIGHT:
        g.pacman.keyHandler[RIGHT] = True
        g.pacman.dir = "RIGHT"
    elif keyCode == UP:
        g.pacman.keyHandler[UP] = True
        g.pacman.dir = "UP"
    elif keyCode == DOWN:
        g.pacman.keyHandler[DOWN] = True
        g.pacman.dir = "DOWN"
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
