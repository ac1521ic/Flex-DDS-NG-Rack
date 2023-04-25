# Flex-DDS-NG-Rack
%matplotlib inline
import numpy as np 
import matplotlib.pyplot as plt
import math as m
import warnings
from sympy.interactive import printing 
import pandas as pd
printing.init_printing(use_latex = (True))
import time

class ScaleFactors:
    
    global afull #full scale amplitude 
    afull = +2
    
    def __init__(self,num,y): #units of num are MHz or dBm
        self.num = num
        self.y = y

    def FTW(self): 
        a = round(self.num*(10**6)*(10**-9)*(2**32))
        if self.y=='hex': return hex(a)  
        if self.y=='dec': return a  
        
    def ASF(self):
        a = (self.num - afull)/20
        b = round((10**a)*((2**14)-1))
        if self.y=='hex': return hex(b)
        if self.y=='dec': return b
        
    def iASF(self): #input in hex , outbut dB
        #warnings.warn("Input needs to be hex or dec value of ASF")
        if isinstance(self.num,str) ==True:
            a = int(str(self.num), 16)
            b = round(a)/(2**14 -1)
            c = m.log(b,10)*20
            return round(c+2)
        elif isinstance(self.num,int) ==True:  
            b = self.num/(2**14 -1)
            c = m.log(b,10)*20
            return round(c+2)

    def iFTW(self): #units of num are hex, output freq(MHz) from FTW
        if isinstance(self.num,str) ==True: return int(str(self.num), 16)*(10**9)*(2**-32)*10**(-6)
        elif isinstance(self.num,int) ==True: return self.num*(10**9)*(2**-32) *10**(-6)
    
    def POW(self): #phase offset word
        return hex(round((self.num*2**16)/360))
    
    def iPOW(self):
        return (int(str(self.num), 16)*360)/(2**16)
        
        
        
