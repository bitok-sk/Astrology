# Astrology
Data_vis
import matplotlib.pyplot as plt
import numpy as np

# === APOPHIS 2029 - The 1000-Year Pass ===
# Date: 2029-04-13 | Distance: 32,000 km | Size: 340m

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(14, 6), dpi=150)
fig.suptitle('Apophis - April 13, 2029 | The 1000-Year Pass', fontsize=14, fontweight='bold')

# --- CHART 1: How close? ---
labels = ['Earth Surface', 'Apophis\n32,000 km', 'GEO Satellites\n36,000 km', 'Moon\n384,400 km']
distances = [0, 32000, 36000, 384400]
colors = ['#2E86AB', '#E63946', '#F4A261', '#A8A8A8']

bars = ax1.bar(labels, distances, color=colors, edgecolor='black')
ax1.set_ylabel('Altitude above Earth (km)')
ax1.set_title('How Close Is It?')
ax1.set_yscale('log')
for bar, d in zip(bars, distances):
    if d>0:
        ax1.text(bar.get_x()+bar.get_width()/2, d*1.2, f'{d:,} km', ha='center', fontweight='bold')

# --- CHART 2: Orbital View ---
theta = np.linspace(0, 2*np.pi, 100)
earth_r = 1
geo_r = 6.6 # 42164km / 6371km earth radius
apophis_r = 6.0 # 38371km / 6371km

ax2.set_aspect('equal')
ax2.fill(np.cos(theta)*earth_r, np.sin(theta)*earth_r, color='#2E86AB', label='Earth')
ax2.plot(np.cos(theta)*geo_r, np.sin(theta)*geo_r, '--', color='#F4A261', label='GEO Satellites')

# Apophis hyperbolic path
approach = np.linspace(-0.8, 0.8, 100)
path_x = (apophis_r + 3*approach**2) * np.cos(approach)
path_y = (apophis_r + 3*approach**2) * np.sin(approach)

ax2.plot(path_x, path_y, color='#E63946', linewidth=3, label='Apophis Path Apr 13')
ax2.plot(path_x[50], path_y[50], 'o', color='#E63946', markersize=12)

ax2.set_xlim(-10, 10)
ax2.set_ylim(-10, 10)
ax2.set_title('Top View: Inside Satellite Belt!')
ax2.legend(fontsize=8)
ax2.axis('off')
ax2.annotate('CLOSER\nTHAN SATELLITES!', xy=(6, 0), xytext=(7.5, 5),
            arrowprops=dict(facecolor='red', shrink=0.05), fontsize=8, fontweight='bold', color='red')

plt.tight_layout()
plt.savefig('apophis_2029.png')
print("Saved: apophis_2029.png")

# --- BONUS: DARK SPACE VERSION ---
fig2, ax = plt.subplots(figsize=(8,8), dpi=200)
fig2.patch.set_facecolor('black')
ax.set_facecolor('black')
ax.set_aspect('equal')

ax.fill(np.cos(theta)*earth_r, np.sin(theta)*earth_r, color='#4A90E2')
ax.plot(np.cos(theta)*geo_r, np.sin(theta)*geo_r, '--', color='orange', alpha=0.6)

# fading trail
for i in range(1, len(path_x)):
    alpha = i / len(path_x)
    ax.plot(path_x[i-1:i+1], path_y[i-1:i+1], color='#FF3333', linewidth=2.5, alpha=alpha)

# stars
np.random.seed(42)
ax.scatter(np.random.uniform(-12,12,100), np.random.uniform(-12,12,100), s=5, color='white', alpha=0.3)

ax.set_xlim(-12, 12)
ax.set_ylim(-12, 12)
ax.axis('off')
ax.set_title('Apophis 2029 Flyby - 32,000 km', color='white', fontweight='bold')

plt.savefig('apophis_dark.png', facecolor='black', bbox_inches='tight')
print("Saved: apophis_dark.png")
plt.show()
