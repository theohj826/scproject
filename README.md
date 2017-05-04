# Final: Tank Attack
# Dongwoo Lee & Theodore Lee

#from random import *
from math import sqrt, pi, radians, sin, cos

#This function calculates the cannonball's landing point   
def cball(angle_degree, vel , wind_vel):
    
    #set initial values
    xpos = 0.0
    ypos = 10.0
    t = 0.01
    df = 0.002
    m = 1.00
    
    # convert angle to radians
    rad = radians(angle_degree)
    
    # get x and y components of velocity
    xvel = vel *cos(rad)
    yvel = vel *sin(rad)
    
    # simulate the launch
    while ypos >= 0:
        
        #yvel2 calc
        yvel_g = - t * 9.8
        yvel_ar = - t * ( (df/m)*abs(((xvel**2)+(yvel**2))**(1/2))*yvel )
        yvel2 = yvel +  yvel_g + yvel_ar   
        
        #xvel 2 calc
        xvel_air_res = - t * ( (df/m)*abs(sqrt((xvel**2)+(yvel**2)))*xvel )
        xvel_wind_load = t * (pi*0.0225)*0.00256*(wind_vel**2)
        if wind_vel < 0:
            xvel_wind_load = -xvel_wind_load
        xvel2 = xvel + xvel_air_res + xvel_wind_load
        
        #Updated Position
        xpos = xpos + t * (xvel + xvel2)/2.0
        ypos = ypos + t * (yvel + yvel2)/2.0
        xvel = xvel2
        yvel = yvel2
    return (xpos)

def main():
    import sys
    import random
    
    #Initial windvel
    wind_vel= random.randint(-50,50)    
    
    #Initial Position
    t1_pos=random.randint(-800,800)
    t2_pos=random.randint(-800,800)
    
    #Minimum distance b/w tanks: 100
    while t1_pos - t2_pos < 100 and t2_pos - t1_pos < 100:
        t1_pos=random.randint(-800,800)
        t2_pos=random.randint(-800,800)
        
    #Intro
    print("\nTank Attack\n\nOBEJCTIVE: DESTROY THE ENEMY TANK\n\nTank Width: 10\nLaunch Angle: 0-180 (degrees)\nLaunch Velocity: 1-1000 (mph)\nDistance Travel: 0-50\nEach turn you may choose to move or shoot\nMove:'m' Shoot:'s'\n")
    print("Type 'end' to quit playing\n\n")
    
    #initial conditions    
    n=0
    turn=1
    while n==0:
        
        #Player 1's turn
        print ("Turn "+str(turn)+"\n\nTank 1")
        print ("Tank 1 position: "+str(t1_pos)+"\nTank 2 position: "+str(t2_pos))
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
            t1_pos=t1_pos+moveby
        if move=="shoot" or move=="s":
            angle_degree= eval(input("Launch Angle: "))
            while angle_degree>180 or angle_degree<0:
                print ("invalid launch angle")
                angle_degree= eval(input("Launch Angle: "))
            vel= eval(input("Launch Velocity: "))
            while vel>1000 or vel<1:
                print ("invalid launch velocity")
                vel= eval(input("Launch Velocity: "))
            t1_shot = int(t1_pos+cball(angle_degree, vel, wind_vel))
            print ("\n\nTank 1's cannonball landed at position "+str(t1_shot)+"\n")
            diff = abs(t1_shot-t2_pos)
            if diff<5:
                n="Tank 1"
                break
            else:
                print("You missed by "+ str(diff))
        
        #Player 2's turn
        print ("\n\nTank 2\n")
        print ("Tank 1 position: "+str(t1_pos)+"\nTank 2 position: "+str(t2_pos))
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
        turn=turn+1
        wind_vel=wind_vel+random.randint(-2,2)
        if wind_vel>50:
            wind_vel=wind_vel-random.randint(3,4)
        if wind_vel<-50:
            wind_vel=wind_vel+random.randint(3,4)
    print (n+" was victorious!")
    return

main()
