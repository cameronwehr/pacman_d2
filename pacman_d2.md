import os

path=os.getcwd()

class Creature:
    def __init__(self,r,y,v,img):
        self.r=r
        self.y=y
        self.v=v
        self.img=loadImage(path+"/images/"+str(self.v)+".png")
        
class PacMan(Creature):
    def __init__(self):
        Creature.__init__(self,x,y,v,img)
        self.keyHandler = {LEFT:False, RIGHT:False, UP:False, DOWN:False}
        
        
class Ghosts(Creature):
    def __init__(self):
        Creature__init__(self,x,y,r,img)
        
class Game:
    def __init__(self,w,h):
        self.w=w
        self.h=h
        self.pacman=PacMan()
        
g = Game(1280,720)

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
