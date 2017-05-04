# scproject

from random import *
#This function calculates the cannonball's landing point     
from math import sin, cos, radians, sqrt, pi
def cannonball(angle_degree, vel_, wind_vel):
    #set initial values
    x_pos = 0.0
    y_pos = 10.0
    time = 0.01
    drag_force = 0.002
    mass_ = 1.00
    # convert angle to radians
    angle_theta= radians(angle_degree)
    # get x and y components of velocity
    x_vel = vel_ *cos(angle_theta)
    y_vel = vel_ *sin(angle_theta)
    # simulate the launch
    while y_pos >= 0:
        #get acceleration and new velocity of x component
        x_vel_air_res = - time * ( (drag_force/mass_)*abs(sqrt((x_vel**2)+(y_vel**2)))*x_vel )
        x_vel_wind_load = time * (pi*0.0225)*0.00256*(wind_vel**2)
        if wind_vel < 0:
            x_vel_wind_load = -x_vel_wind_load
        x_vel_new = x_vel + x_vel_air_res + x_vel_wind_load
        #get acceleration and new velocity of y component
        y_vel_gravity = - time * 9.8
        y_vel_air_res = - time * ( (drag_force/mass_)*abs(((x_vel**2)+(y_vel**2))**(1/2))*y_vel )
        y_vel_new = y_vel +  y_vel_gravity + y_vel_air_res
        #calculate new positions
        x_pos = x_pos + time * (x_vel + x_vel_new)/2.0
        y_pos = y_pos + time * (y_vel + y_vel_new)/2.0
        x_vel = x_vel_new
        y_vel = y_vel_new
    return (x_pos)

def main():
    #randomly assign initial positions
    t1_pos=randint(-1000,1000)
    t2_pos=randint(-1000,1000)
    #make sure that the tanks are not too close together
    while t1_pos - t2_pos < 200 and t2_pos - t1_pos < 200:
        t1_pos=randint(-1000,1000)
        t2_pos=randint(-1000,1000)
    #set initial random wind
    wind_vel= randint(-50,50)
    #game start message
    print("Tank Attack\n\nShoot the other tank to win\nThe tanks are 10 spaces wide\nSet launch angles between 0-180 degrees\nSet launch velocities between 1-1000 mph\nYou may choose to move the tank instead of shoot\nMove tank by up to 50 units each turn\n\n")
    #initial conditions    
    victor="NA"
    turn=1
    while victor=="NA":
        
        #Player 1's turn
        print ("Turn "+str(turn)+"\n\nTank 1")
        print ("Tank 1 position: "+str(t1_pos)+"\nTank 2 position: "+str(t2_pos))
        print ("Wind: "+str(wind_vel)+"mph")
        move_=input("Will you move or shoot? (move/shoot)\n")
        while move_!="move" and move_!="m" and move_!="shoot" and move_!="s":
            print("Invalid input")
            move_=input("Will you move or shoot? (move/shoot)\n")
        if move_=="move" or move_=="m":
            move_by=eval(input("Move tank by: "))
            while move_by>50 or move_by<-50:
                print ("Invalid move increment")
                move_by=eval(input("Move tank by: "))
            t1_pos=t1_pos+move_by
        if move_=="shoot" or move_=="s":
            angle_degree= eval(input("Launch Angle: "))
            while angle_degree>180 or angle_degree<0:
                print ("invalid launch angle")
                angle_degree= eval(input("Launch Angle: "))
            vel_= eval(input("Launch Velocity: "))
            while vel_>1000 or vel_<1:
                print ("invalid launch velocity")
                vel_= eval(input("Launch Velocity: "))
            t1_shot = int(t1_pos+cannonball(angle_degree, vel_, wind_vel))
            print ("\n\nTank 1's cannonball landed at position "+str(t1_shot)+"\n\n\n")
            if t1_shot-t2_pos<5 and t2_pos-t1_shot<5:
                victor="Tank 1"
                break
        #Player 2's turn
        print ("Tank 2\n")
        print ("Tank 1 position: "+str(t1_pos)+"\nTank 2 position: "+str(t2_pos))
        print ("Wind: "+str(wind_vel)+"mph")
        move_=input("Will you move or shoot? (move/shoot)\n")
        while move_!="move" and move_!="m" and move_!="shoot" and move_!="s":
            print("Invalid input")
            move_=input("Will you move or shoot? (move/shoot)\n")
        if move_=="move" or move_=="m":
            move_by=eval(input("Move tank by: "))
            while move_by>50 or move_by<-50:
                print ("Invalid move increment")
                move_by=eval(input("Move tank by: "))
            t2_pos=t2_pos+move_by
        if move_=="shoot" or move_=="s":
            angle_degree= eval(input("Launch Angle: "))
            while angle_degree>180 or angle_degree<0:
                print ("invalid launch angle")
                angle_degree= eval(input("Launch Angle: "))
            vel_= eval(input("Launch Velocity: "))
            while vel_>1000 or vel_<1:
                print ("invalid launch velocity")
                vel_= eval(input("Launch Velocity: "))
            t2_shot = int(t2_pos+cannonball(angle_degree, vel_, wind_vel))
            print ("\n\nTank 2's cannonball landed at position "+str(t2_shot)+"\n\n\n")
            if t2_shot-t1_pos<5 and t1_pos-t2_shot<5:
                victor="Tank 2"
            
        #change things for the next turn
        turn=turn+1
        wind_vel=wind_vel+randint(-2,2)
        if wind_vel>50:
            wind_vel=wind_vel-randint(3,4)
        if wind_vel<-50:
            wind_vel=wind_vel+randint(3,4)
    print (victor+" was victorious!")
    return

main()
