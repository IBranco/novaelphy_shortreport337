# TEMPLATE - EMG Event Counter

## Goal

Use Bitalino to acquire an electromyogram, EMG signal, and estimate how many sequential activations are present in a signal.

## Team

* Ines Branco 42368
* Marcio Gomes 42757
* Rita Joao 42344
* Tomas Correia 42352

## Process

We passed by the several steps:

### Signal recorded

In the first step we've recorded data. 

The first signal is the signal extracted from bitalino. The second signal is with the correct axis in mV. The third signal is the second signal with filter.

![signal](https://fbcdn-sphotos-h-a.akamaihd.net/hphotos-ak-prn2/v/t35.0-12/10337383_833309313363097_1739333400_o.jpg?oh=f69e51b5d4db8abef7bfd67442687623&oe=5372E20B&__gda__=1400055950_259e45827a1d76855db73fb5d1bb81c4)

We concluded that we needed to use a better electrodes position

## Code example


from pylab import *
import novainstrumentation as ni
from scipy import *


s = loadtxt('EMG_Rita_5Cont.txt')
EMGb=s[:,-1]
t = arange(len(EMGb))/1000.

subplot(3,1,1)
title('Sinal inicial')
xlabel('Sample #')
ylabel('ADC')
plot(t, EMGb, color='pink')
show()

nbits=10
Vcc=3.3

G=1000.

EMGv = (EMGb/(2**nbits-1)-.5)*Vcc/2./G
EMGmv = (EMGv-EMGv.mean())*G

subplot(3,1,2)
title('Sinal sem filtro')
xlabel('t(s)')
ylabel('mV')
plot(t, EMGmv, color='orange')
show()

subplot(3,1,3)
ft = ni.filter.bandpass(EMGmv,1.,10.,fs=1000)
title('Sinal Com Filtro Passa Baixo')
xlabel('t(s)')
ylabel('mV')
plot(t,ft, color='green')
show()

figure()
cena=ni.code.smooth(abs(EMGmv),1000)
plot(t, cena)
show()

time_beats=[]

picos=ni.peaks(ft, tol=0.01145)
for i in picos:
    beat=t[i]
    time_beats.append(beat)
beats_timer=diff(time_beats)
print "Numero de Contraccoes:    " + str(len(beats_timer)) + "[Contraccoes]"


The complete code examples can be found on the github link:

[Github page](https://github.com/hgamboa/novainstrumentation)



## Take ways
The major discoveries made in this project were: 

* Scientific enlightment 
