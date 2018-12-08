import os

path=os.getcwd()

class Block:
    def __init__(self,x,y):
        self.x=x
        self.y=y
        self.img = loadImage(path+"/images/block.png")
        
    def display(self):
        image(self.img,self.x*20,self.y*20)
            
class Pellet:
    def __init__(self,x,y,v=0): 
        #v is value for score for later
        self.x=x
        self.y=y
        self.v=v
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
                # elif i == '*':
                #     self.blocks.append(Block(x*20,y*20,'special'))
                x+=1
            y+=1
            
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
        self.dir = "RIGHT"
        
        
    def update(self):
        if self.keyHandler[LEFT]:
            self.vx = -10
            self.dir = 'LEFT'
            print(self.dir)
        elif self.keyHandler[RIGHT]:
            self.vx = 10
            self.dir = 'RIGHT'
            print(self.dir)
        elif self.keyHandler[UP]:
            self.vy = -10
            self.dir = 'UP'
            print(self.dir)
        elif self.keyHandler[DOWN]:
            self.vy = 10
            self.dir = 'DOWN'
            print(self.dir)
        else:
            self.vy=0
            self.vx=0
        
        # before we add hte velocities, check if PacMan is in a square that lets him move to the next block
        nextBlockSafe = self.checkNextTile()
        if nextBlockSafe:
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
        image(self.img,self.x,self.y,20,20)
        self.update()
        
    # True if moveable, False if block
    def checkNextTile(self):
        # 1. Convert pacman's x and y coordinates to a row and column
        checkR = self.y//20
        checkC = self.x//20

        # # 2 Based on the direction pacman is facing, get the tile that is +1 or -1 to r or c, depending
        if self.dir == 'RIGHT':
            checkC = checkC+1
        if self.dir == 'LEFT':
            checkC = checkC-1
        if self.dir == 'DOWN':
            checkR = checkR+1
        if self.dir == 'UP':
            checkR = checkR-1
        print("Checking {},{}".format(checkR, checkC))
            
            
        for b in bo.blocks:
            if b.y==checkR or b.x==checkC:
                return False
        return True
    


        
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
        
        
bo = Board()
g = Game(540,540)

def setup():
    size(g.w,g.h)
    
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
