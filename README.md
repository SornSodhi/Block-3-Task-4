# Block-3-Task-4

import matplotlib.pyplot as plt
from scipy.integrate import odeint
import sys
import numpy as np

initial_flag = '--initial' in sys.argv
alpha_flag = '--alpha' in sys.argv
beta_flag = '--beta' in sys.argv
delta_flag = '--delta' in sys.argv
gamma_flag = '--gamma' in sys.argv
save_plot = '--save_plot' in sys.argv

x0 = 10  
y0 = 10  
alpha = 1.1  
beta = 0.4   
delta = 0.1  
gamma = 0.4 

if alpha_flag:
    alpha_input = input("Enter new values for alpha:")
    alphas = [float(value) for value in alpha_input.split()][:5]
else:
    alphas = [1.1]
if '--beta' in sys.argv:
    beta = float(input("Enter new value for beta: "))
if '--delta' in sys.argv:
    delta = float(input("Enter new value for delta: "))
if '--gamma' in sys.argv:
    gamma = float(input("Enter new value for gamma: "))
if '--initial' in sys.argv:
    x0 = float(input("Enter new value for initial Prey number: "))
    y0 = float(input("Enter new value for initial predator number: "))

def diff_lotka_volterra(xy, t, alpha, beta, delta, gamma):
    x, y = xy
    dxdt = alpha * x - beta * x * y
    dydt = delta * x * y - gamma * y
    return [dxdt, dydt]

t = np.linspace(0, 50, 1000)  
xy0 = [x0, y0] 

plt.figure(figsize=(12, 5))
plt.subplot(1, 2, 1)

for i,alpha in enumerate(alphas):
    solution = odeint(diff_lotka_volterra, [x0, y0], t, args=(alpha, beta, delta, gamma))
    plt.plot(t, solution[:, 0], label=f'Prey, Alpha={alpha}')
    plt.plot(t, solution[:, 1], label=f'Predator, Alpha={alpha}')
    plt.legend()
    plt.xlabel('Time')
    plt.ylabel('Population')
    plt.title('Lotka-Volterra Model')
    if '--save_plot' in sys.argv:
        plt.savefig('lotka_volterra_model.png')
    else:
        plt.show()
