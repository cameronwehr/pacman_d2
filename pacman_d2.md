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
    def __init__(self,x,y,name,v=1,vx=0,vy=0):       # add attribute name if want to have ghost name
        self.x=x
        self.y=y
        self.v=v
        self.dir='RIGHT'
        self.imgRight=loadImage(path+'/images/'+name+'RIGHT.png')
        self.imgLeft=loadImage(path+'/images/'+name+'LEFT.png')
        self.imgUp=loadImage(path+'/images/'+name+'UP.png')
        self.imgDown=loadImage(path+'/images/'+name+'DOWN.png')
        
 
    def display(self):
        self.update()
        if self.dir == 'RIGHT':
            image(self.imgRight,self.x,self.y,self.dim,self.dim)
        elif self.dir == 'LEFT':
            image(self.imgLeft,self.x,self.y,self.dim,self.dim)
        elif self.dir == 'UP':
            image(self.imgUp,self.x,self.y,self.dim,self.dim)
        elif self.dir == 'DOWN':
            image(self.imgDown,self.x,self.y,self.dim,self.dim)
        
class PacMan(Creature):
    def __init__(self,x,y,name):
        Creature.__init__(self,x,y,name)
        self.keyHandler = {LEFT:False, RIGHT:False, UP:False, DOWN:False}
        self.vx=0
        self.vy=0
        self.dim = 20
        
        
    def update(self):
        if self.keyHandler[LEFT]:
            self.vx = -20
            self.vy = 0
            self.dir = 'LEFT'
        elif self.keyHandler[RIGHT]:
            self.vx = 20
            self.vy = 0
            self.dir = 'RIGHT'
        else:
            self.vx = 0 
        
        if self.keyHandler[UP]:
            self.vx = 0
            self.vy = -20
            self.dir = 'UP'
        elif self.keyHandler[DOWN]:
            self.vx = 0
            self.vy = 20
            self.dir = 'DOWN'
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
                
                # if (b.x <= self.x+b.dim <= b.x + b.dim) and (b.y - b.dim < self.y < b.y +  b.dim):
                #     return False
        # if self.dir == 'DOWN':
        #     for b in g.board.blocks:
        #         if (b.x - b.dim < self.x < b.x + b.dim) and (b.y - b. dim <=self.y <= b.y):
        #             return False        
        # if self.dir == 'UP':
        #     for b in g.board.blocks:
        #         if (b.x - b.dim < self.x < b.x + b.dim) and (b.y <= self.y <= b.y + b.dim ):
        #             return False    
        # print(self.x, self.y)
      
        return True   
                
                
'''   
            #return True 
        #     print("CAST FROM TOPLEFT AND BOTTOMLEFT IN LEFT DIRECTION")
        elif self.dir == 'RIGHT':
             checkC = checkC +1
             
             
        #     print("CAST FROM TOPRIGHT AND BOTRIGHT IN RIGHT DIRECTION")
        elif self.dir == 'UP':
            checkR = checkR -1 
        #     print("CAST FROP TOPLEFT AND TOPRIGHT IN UP DIRECTION")
        elif self.dir == 'DOWN':
            checkR = checkR -1 
        
        return True
        #     print("CAST FROP BOTLEFT AND BOTRIGHT IN DOWN DIRECTION")
        # print("IF BOTH CASTS GAVE EMPTY, WE CAN MOVE IN THAT DIRECTION")
        # else:
        #     return False
    
        
    
'''

        
class Game:
    def __init__(self,w,h):
        self.w=w
        self.h=h
        self.pacman=PacMan(20,20,'pacman')
        
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
