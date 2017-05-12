# Final: Tank Attack
# Dongwoo Lee & Theodore Lee

#from random import *
from math import sqrt, pi, radians, sin, cos

#This function calculates the cannonball's landing point   
class canball:
    def __init__(self,xpos,angle_degree, vel , wind_vel):
        self.t = 0.01
        self.d = 0.0002
        self.m = 1.00
        self.wind = wind_vel
        self.rad = radians(angle_degree)
        
        self.xpos = xpos
        self.ypos = 10.0
        self.xvel = vel*cos(self.rad)
        self.yvel = vel*sin(self.rad)
        
        self.x=[]
        self.y=[]
        
    def update(self,time):
        v_mag=sqrt(self.yvel**2+self.xvel**2)
        g_y = -9.8*time
        ar_y = -(self.d/self.m)*v_mag*self.yvel*time
        yvel2 = self.yvel + g_y + ar_y
        
        ar_x = -(self.d/self.m)*v_mag*self.xvel*time
        wind_x = (pi*0.0225)*0.00256*(abs(self.wind)**2)*time #check this code
        xvel2 = self.xvel + ar_x + wind_x
        
        self.xpos = self.xpos + time * (self.xvel + xvel2) / 2.0
        self.ypos = self.ypos + time * (self.yvel + yvel2) / 2.0
        self.yvel = yvel2
        self.xvel = xvel2
        self.x.append(self.xpos)
        self.y.append(self.ypos)
        
    def get_y(self):
        return (self.ypos)
    
    def get_x(self):
        return (self.xpos)
    
class tank:
    def __init__(self,pos):
        self.pos = pos
    
    def move(self,dist):
        self.pos = self.pos + dist

    def getpos(self):
        return(self.pos)
    
def calculate (cball):
    while cball.get_y() >= 0:
        cball.update(0.001)
    return (cball.get_x())

def main():
    import sys
    import random
    
    #Initial windvel
    wind_vel= random.randint(-50,50)    
    
    #Initial Position
    t1_pos=random.randint(-800,800)
    t2_pos=random.randint(-800,800)
    
    #Minimum distance b/w tanks: 100
    while abs(t1_pos - t2_pos) < 100:
        t1_pos=random.randint(-800,800)
        t2_pos=random.randint(-800,800)
    
    tank1 = tank(float(t1_pos))
    tank2 = tank(float(t2_pos))
    
    #Intro
    print("***********\nTank Attack\n\nOBEJCTIVE: DESTROY THE ENEMY TANK\n\nTank Width: 10\nLaunch Angle: 0-180 (degrees)\nLaunch Velocity: 1-1000 (mph)\nDistance Travel: 0-50\nEach turn you may choose to move or shoot\nMove:'m' Shoot:'s'\n")
    print("Type 'end' to quit playing\n*********************\n")
    
    #initial conditions    
    n=0
    Round=1
    while n==0:
        
        #Player 1's turn
        print ("Round "+str(Round)+"\n\n")
        print ("****Tank 1 Turn****\n\nTank 1 position: "+str(tank1.getpos())+"\nTank 2 position: "+str(tank2.getpos()))
        print ("Wind: "+str(wind_vel)+" mph")
        move=input("Move or Shoot?\n")
        if move == "end":
            sys.exit()
        while move!="move" and move!="m" and move!="shoot" and move!="s":
            print("Invalid input")
            move=input("Move or Shoot? ('m'/'s')\n")
            if move == "end":
                sys.exit()
                
        #player 1 move
        if move=="move" or move=="m":
            dist=eval(input("Move tank by: "))
            while abs(dist) > 50:
                print ("Invalid move increment")
                dist=eval(input("Move tank by: "))
            tank1.move(dist)
            
        #player 1 shoot
        if move=="shoot" or move=="s":
            angle_degree= eval(input("Launch Angle: "))
            while angle_degree>180 or angle_degree<0:
                print ("invalid launch angle")
                angle_degree= eval(input("Launch Angle: "))
            vel= eval(input("Launch Velocity: "))
            while vel>1000 or vel<1:
                print ("invalid launch velocity")
                vel= eval(input("Launch Velocity: "))
            cball = canball(tank1.getpos(),angle_degree, vel , wind_vel)
            t1_shot = calculate(cball)
            print ("\n\nTank 1's cannonball landed at position "+str(t1_shot)+"\n")
            diff = abs(t1_shot-tank2.getpos())
            if diff<5:
                n="Tank 1"
                break
            else:
                print("You missed by "+ str(diff))
        
        #Player 2's turn
        print ("\n\nTank 2\n")
        print (tank1.pos)
        print ("Tank 1 position: "+str(tank1.getpos())+"\nTank 2 position: "+str(tank2.getpos()))
        print ("Wind: "+str(wind_vel)+" mph")
        move=input("Move or Shoot?\n")
        if move == "end":
            sys.exit()
        while move!="move" and move!="m" and move!="shoot" and move!="s":
            print("Invalid input")
            move=input("Move or Shoot? ('m'/'s')\n")
            if move == "end":
                sys.exit()
        if move=="move" or move=="m":
            moveby=eval(input("Move tank by: "))
            while moveby>50 or moveby<-50:
                print ("Invalid move increment")
                moveby=eval(input("Move tank by: "))
            t2_pos=t2_pos+moveby
            
        #player 2 shoot
        if move=="shoot" or move=="s":
            angle_degree= eval(input("Launch Angle: "))
            while angle_degree>180 or angle_degree<0:
                print ("invalid launch angle")
                angle_degree= eval(input("Launch Angle: "))
            vel= eval(input("Launch Velocity: "))
            while vel>1000 or vel<1:
                print ("invalid launch velocity")
                vel= eval(input("Launch Velocity: "))
            t2_shot = int(t2_pos+cball(angle_degree, vel, wind_vel))
            print ("\n\nTank 2's cannonball landed at position "+str(t2_shot)+"\n\n\n")
            diff = abs(t2_shot-t1_pos)
            if diff<5:
                n="Tank 2"
                break
            else:
                print("You missed by "+ str(diff))
            
        #change things for the next turn
        Round=Round+1
        wind_vel=wind_vel+random.randint(-2,2)
        if wind_vel>50:
            wind_vel=wind_vel-random.randint(3,4)
        if wind_vel<-50:
            wind_vel=wind_vel+random.randint(3,4)
        
    print ("******************************HIT*HIT*HIT*HIT*HIT*HIT*HIT*HIT******************************")
    print (n+" won!")
    return

main()
