# Tarea1_Estadisitica
import math
import numpy as np
import random
import matplotlib.pyplot as plt




def indexOf(array,number):
    for i in xrange(0,len(array)):
        if array[i]==number:
            return i
    return -1

def nCr(n,r):
    f=math.factorial
    return f(n)/f(r)/f(n-r)

def votacion(n,r):
    V=range(n)
    rand=0
    for i in xrange(0,n):
        rand=random.uniform(0,1)
        if rand<=r:
            V[i]=1.
        if rand>r:
            V[i]=0.

    return V  ##V es un arreglo que en cada casilla tiene 1 con probabilidad r y 0 con probabilidad 1-r

def Binomial(n,x,p):
    return nCr(n,x)*(p**x)*(1.-p)**(n-x)

def Varias_votaciones(nvot,nper,r):
    Resultado=range(nvot)
    for i in xrange(0,nvot):
        aux=votacion(nper,r)
        Resultado[i]=sum(aux)

    nbins=max(Resultado)-min(Resultado)
    plt.hist(Resultado,nbins)
    plt.title('Histograma de 1000 votaciones')
    plt.savefig('Histograma1000.png')
    plt.show()
    
    return Resultado



def integral(x,deltaR,n):
    r=0
    sum=0
    while r<=1:
        sum=sum+Binomial(n,x,r)*deltaR
        r=r+deltaR
        

    return sum




def pdfRdadoX(n,x,r,deltaR):
    
    numerador=Binomial(n,x,r)
    denominador=integral(x,deltaR,n)
    out=numerador/denominador
    return out


def CDF(array_pdf,r):
    delta=r[1]-r[0]
    n=len(array_pdf)
    Out=range(n)
    for i in xrange(0,n):
        Out[i]=0
    Out[0]=array_pdf[0]
    for i in xrange(1,n):
        Out[i]=Out[i-1]+array_pdf[i]*delta
    return Out

def pdfGen(N,n,x,deltaR):
    pdf=range(N)
    rvec=np.linspace(0,1,N)
    for i in xrange(0,N):
        pdf[i]=pdfRdadoX(n,x,rvec[i],deltaR)
    return [rvec,pdf]
    
    
def plotpdf(N,n,x,deltaR):
    H=pdfGen(N,n,x,deltaR)
    f=H[1]
    r=H[0]
    delta=r[1]-r[0]
    plt.title('Probability density function for r, with a given x')
    plt.ylabel('Probability density')
    plt.xlabel('value for r')
    plt.plot(r,f)
    plt.savefig('pdf.png')
    plt.show()
    sumsup=0
    suminf=0
    for i in xrange(0,len(f)):
        if r[i]<0.5:
            suminf=suminf+f[i]*delta
            print str(r[i])+'      '+str(f[i])
        if r[i]>0.5:
            sumsup=sumsup+f[i]*delta
            print str(r[i])+'      '+str(f[i])
    print 'Probabilidad de que r>0.5= '+str(sumsup)
    print 'Probabilidad de que r<0.5= '+str(suminf)
    maximo=max(f)
    index=indexOf(f,maximo)
    print 'El r de mayor probabilidad es '+str(r[index])
    
        
    return
def plotcdf(N,n,x,deltaR):
     H=pdfGen(N,n,x,deltaR)
     f=H[1]
     r=H[0]
     cdf=CDF(f,r)
     plt.title('Cumulative density function for r, with a given x')
     plt.xlabel('value for r')
     plt.ylabel('Cumulative probability density')
     plt.plot(r,cdf)
     plt.savefig('cdf.png')
     
     plt.show()
     
     return




   
    
a=Varias_votaciones(10000,33,0.5)
plotcdf(N,n,x,deltaR)
plotpdf(200,33,18,0.0001)

