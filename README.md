# Spur-Gear-Design
#importing libraries
import numpy as np


tp = int(input('No of teeth on pinion = '))
tg = int(input('No of teeth on gear = '))
z = int(tp + tg)
Sut_p = int(input('Beam strength of pinion = '))
s_p = Sut_p / 3
Sut_g = int(input('Beam strength of gear = '))
s_g = Sut_g / 3
Y = 0.484 - 2.865/z
Q = 2*tg / z
BHN = int(input('Enter BHN ='))
K = 0.16*(BHN/100)**2
Ka = float(input('Service factor = '))
Km = float(input('Load Distribution Factor = '))
b = 10 
FOS = int(input('FOS = '))
N = int(input('Speed = '))
p = int(input('Power in watts = '))

# determining beam strength
beam_strength_pinion = ((s_p)*(b)*(Y))
print(f'This is the beam strength of pinion {beam_strength_pinion}N')
beam_strength_gear = ((s_g)*(b)*(Y))
print(f'This is the beam strength of gear {beam_strength_gear}N')

if beam_strength_gear > beam_strength_pinion:
    print('pinion is weaker in bending')
    
else:
    print('gear is weaker in bending')
    
#determining wear strength

wear_strength = (tp)*(b)*(Q)*(K)

#selecting the lowest of all

lowest_value = min(wear_strength, beam_strength_pinion, beam_strength_gear)  
print(f'This is the value we will consider {lowest_value}N')

#calculating velocity

v = (3.14 * tp * N) / 60

#calculating effective load by step 1

f_eff = lowest_value / FOS

#calculating effective load by step 2
#calculating f-tmax
#calculating f-t

f_t = p / v
f_tmax = (f_t)*(Ka)*(Km)

#defining coefficients

A = int(f_eff)
B = 0
C = int(f_tmax * v)
D = (f_tmax)

roots = np.roots([A,B,-C,-D])
print(f'These are the values of modules {roots}')

for root in roots:
    if root > 0:
        module = round(root)
        print(f'module = {module}')
        
#calculating the values
width = b * module
print(f' width = {width}')

Ft_max =  p / (v * module)
print(f'maximum tangential force = {Ft_max}N')

Wear_strength = (tp)*(b)*(Q)*(K) * (module ** 2)
print(f'Wear strength = {Wear_strength}')

Dp = tp * module
print(f'Diameter of pinion  = {Dp}mm')

Dg = tg * module
print(f'Diameter of gear = {Dg}mm ')

velocity = (v * module)/1000
print(f'Velocity = {velocity} metres per second')
