import os

path=os.getcwd()

class Block:
    def __init__(self,x,y,v):
        self.x=x
        self.y=y
        self.v=v
        self.img = loadImage(path+"/images/"+str(self.v)+".png")
        
    def display(self):
        if self.v == 'block':
            image(self.img,self.x,self.y)
        elif self.v == 'pellet':
            image(self.img,self.x+8,self.y+8,5,5)
        # elif self.v == 'special':
        #     image(self.img,self.x,self.y)

class Board:
    def __init__(self):
        self.blocks = []
        inputFile = open(path+"/PacMan-board.csv","r")
        y = 0
        for line in inputFile:
            line = line.strip().split(",")
            
            x = 0
            for i in line:
                if i == '#':
                    self.blocks.append(Block(x*20,y*20,'block'))
                elif i == '.':
                    self.blocks.append(Block(x*20,y*20,'pellet'))
                # elif i == '*':
                #     self.blocks.append(Block(x*20,y*20,'special'))
                x+=1
            y+=1
            
    def display(self):
        for block in self.blocks:
            block.display()
        

class Creature:
    def __init__(self,x,y,r,v=1,vx=0,vy=0):
        self.x=x
        self.y=y
        self.r=r
        self.v=v
        self.dir = 0
        self.img=loadImage(path+"/images/pacman.png")
    
    # display(self):
    #     if self.dir == 1:
    #         image(self.img,self.x-self.w//2-g.x,self.y-self.h//2,self.w,self.h,int(self.f)*self.w,0,(int(self.f)+1)*self.w,self.h)
    #     elif self.dir == 2:
    #         image(self.img,self.x-self.w//2-g.x,self.y-self.h//2,self.w,self.h,int(self.f+1)*self.w,0,int(self.f)*self.w,self.h)
    #     elif self.dir == 3:
    #         image()
    #     elif self.dir == 4:
    #         image()
     
        
class PacMan(Creature):
    def __init__(self,x,y,r):
        Creature.__init__(self,x,y,r)
        self.keyHandler = {LEFT:False, RIGHT:False, UP:False, DOWN:False}
        self.vx=0
        self.vy=0
        
    def update(self):
        if self.keyHandler[LEFT]:
            self.vx = -10
            self.dir = 1
        elif self.keyHandler[RIGHT]:
            self.vx = 10
            self.dir = 2
        elif self.keyHandler[UP]:
            self.vy = -10
            self.dir = 3
        elif self.keyHandler[DOWN]:
            self.vy = 10
            self.dir = 4
        else:
            self.vy=0
            self.vx=0
        
        self.x += self.vx
        self.y += self.vy

    def display(self):
        image(self.img,self.x,self.y,20,20)
        self.update()
        
    def getTile(self,r,c):
        for b in Board.blocks:
            if b.row==r and b.column==c:
                return b
        return False
    
    def distance(self,e):
        return ((self.x-e.x)**2 + (self.y-e.y)**2)**0.5
    
#        if self.dir == 1:
#            image(self.img,self.x-self.w//2-g.x,self.y-self.h//2,self.w,self.h,int(self.f)*self.w,0,(int(self.f)+1)*self.w,self.h)
#        elif self.dir == 2:
#            image(self.img,self.x-self.w//2-g.x,self.y-self.h//2,self.w,self.h,int(self.f+1)*self.w,0,int(self.f)*self.w,self.h)
        # elif self.dir == 3:
        #     image()
        # elif self.dir == 4:
        #     image()


        
class Game:
    def __init__(self,w,h):
        self.w=w
        self.h=h
        self.pacman=PacMan(20,20,10)
        self.board = Board()
        
    def display(self):
        self.pacman.display()
        self.board.display()
        
        

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
        
def keyReleased():
    if keyCode == LEFT:
        g.pacman.keyHandler[LEFT] = False
    elif keyCode == RIGHT:
        g.pacman.keyHandler[RIGHT] = False
    elif keyCode == UP:
        g.pacman.keyHandler[UP] = False
    elif keyCode == DOWN:
        g.pacman.keyHandler[DOWN] = False
