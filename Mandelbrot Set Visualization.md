```python
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import interact, FloatSlider

# Define the dimensions of the image
width, height = 800, 800
max_iter = 100  # Maximum number of iterations

def compute_mandelbrot(xmin, xmax, ymin, ymax, width=800, height=800, max_iter=100):
    """Compute the Mandelbrot set for a given range."""
    x_vals = np.linspace(xmin, xmax, width)
    y_vals = np.linspace(ymin, ymax, height)
    X, Y = np.meshgrid(x_vals, y_vals)
    c = X + 1j * Y

    mandelbrot = np.zeros_like(c, dtype=int)
    z = np.zeros_like(c)
    
    for i in range(max_iter):
        z = z**2 + c
        mask = np.abs(z) <= 2
        mandelbrot += mask
        
    return mandelbrot

# Define the interactive plot function
def interactive_plot(x_center=-0.5, y_center=0, zoom_level=1.0):
    """Interactive plot for Mandelbrot set."""
    span = 3.0 / zoom_level
    xmin, xmax = x_center - span, x_center + span
    ymin, ymax = y_center - span, y_center + span

    mandelbrot = compute_mandelbrot(xmin, xmax, ymin, ymax)
    
    fig, ax = plt.subplots(figsize=(8,8))
    img = ax.imshow(np.log(mandelbrot + 1), cmap="inferno", extent=(xmin, xmax, ymin, ymax))
    plt.colorbar(img, ax=ax, label="Log Iterations")
    
    def on_scroll(event):
        """Zoom in or out of the plot."""
        nonlocal zoom_level, x_center, y_center
        zoom_scale = 1.5
        if event.button == "up":
            zoom_level /= zoom_scale
        elif event.button == "down":
            zoom_level *= zoom_scale
        interactive_plot(x_center, y_center, zoom_level)
        
    def on_click(event):
        """Center the plot on the clicked position."""
        nonlocal x_center, y_center
        x_center, y_center = event.xdata, event.ydata
        interactive_plot(x_center, y_center, zoom_level)
    
    fig.canvas.mpl_connect('scroll_event', on_scroll)
    fig.canvas.mpl_connect('button_press_event', on_click)
    plt.show()

# Create interactive sliders
interact(interactive_plot,
         x_center=FloatSlider(min=-2, max=1, step=0.01, value=-0.5, description="X Center"),
         y_center=FloatSlider(min=-1.5, max=1.5, step=0.01, value=0, description="Y Center"),
         zoom_level=FloatSlider(min=1, max=100, step=0.5, value=1, description="Zoom Level"))

```


    interactive(children=(FloatSlider(value=-0.5, description='X Center', max=1.0, min=-2.0, step=0.01), FloatSlid…





    <function __main__.interactive_plot(x_center=-0.5, y_center=0, zoom_level=1.0)>




```python

```
