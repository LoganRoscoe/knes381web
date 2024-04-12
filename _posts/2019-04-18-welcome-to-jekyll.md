---
title: "Python Coding"
date: 2019-04-18T15:34:30-04:00
categories:
  - blog
tags:
  - Jekyll
  - update
---

The code displayed demonstrates the construction of two graphs. The first displays VO2, VE, and VCO2 plotted over time. The second graph displays VCO2, FECO2, and FEO2 plotted over VO2 with the appropriate ventilatory exhange thresholds. 

'''

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import matplotlib.pyplot as plt


df = pd.read_csv('../input/demo-knes381/subject_1232.csv', header=[0], skiprows=[1,2,3])

df = df.rename(columns={'VE/': 'VE/VO2','VE/.1': 'VE/VCO2'})

x = df['TIME']
y = df['VO2']
y1 = df['VE']
y2 = df['VCO2']
y3 = df['FEO2']
y4 = df['FECO2']
 
ymax = max(y)

xmax = x[y.argmax()]

fig, ax = plt.subplots(3, 1, sharex=True, figsize=(8, 10)) # Note I increased the figure size here.

fig.subplots_adjust(hspace=0)

ax[0].annotate('$\dot VO_2max$ =({}) L/min'.format(round(ymax, 2)), 
               xy=(xmax, ymax), xytext=(xmax+.5, ymax+ .5),
               arrowprops=dict(facecolor='red', shrink= 0.05),
                )

ax[0].plot(x, y, label=('$\dot VO_2$'), c='r', linestyle='-' )

ax[0].spines[['right', 'top']].set_visible(False)
ax[0].set(ylabel=('L/min'))
ax[0].set(xlabel=('Time(min)'))
ax[0].legend()


ax[1].plot(x, y1, label=('VE'), c='b', linestyle='-')
ax[1].spines[['top', 'right']].set_visible(False)
ax[1].set(ylabel=('breaths/min'))
ax[1].set(xlabel=('Time(min)'))
ax[1].legend()


ax[2].plot(x, y2, label=('VCO2'), c='g',linestyle='-')
ax[2].spines[['top', 'right']].set_visible(False)
ax[2].set(ylabel=('L/min'))
ax[2].set(xlabel=('Time(min)'))
ax[2].legend()

fig.savefig("VO2-VE-4.png", dpi=300, bbox_inches = "tight")
fig.show()

fig, ax = plt.subplots(3, 1, sharex=True, figsize=(8, 10))  # Increased figure size for the third subplot

fig.subplots_adjust(hspace=0)

ax[0].plot(y, y4, 'o', label=('FEO2'), c='r')
ax[0].spines[['top', 'right']].set_visible(False)
ax[0].set(ylabel=('BPM'))
ax[0].set(xlabel=('VO2(L/min)'))
ax[0].legend()

ax[1].plot(y, y3, 'o', label=('FECO2'), c='b')
ax[1].spines[['top', 'right']].set_visible(False)
ax[1].set(ylabel=('%'))
ax[1].set(xlabel=('VO2(L/min)'))
ax[1].legend()

ax[2].plot(y, y2, 'o', label=('VCO2'), c='g')
ax[2].spines[['top', 'right']].set_visible(False)
ax[2].set(ylabel=('%'), xlabel=('VO2(L/min)'))
ax[2].legend()

ax[0].axvline(x=2, color='m', linestyle='-', label='Gas Exchange Threshold')
ax[0].axvline(x=3.685, color='m', linestyle='-', label='Respiratory Compensation')
ax[1].axvline(x=2, color='m', linestyle='-', label='Gas Exchange Threshold')
ax[1].axvline(x=3.685, color='m', linestyle='-', label='Respiratory Compensation')
ax[2].axvline(x=2, color='m', linestyle='-', label='Gas Exchange Threshold')
ax[2].axvline(x=3.685, color='m', linestyle='-', label='Respiratory Compensation')

fig.savefig("VO2-VE-FECO2-VCO2.png", dpi=300, bbox_inches="tight")
fig.show()
'''

