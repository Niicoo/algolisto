Matplotlib Widgets
==================

A lot of widgets can be added in matplotlib figures to interact and dynamically update the plot. You can see the full list of widgets available `here <https://matplotlib.org/stable/gallery/widgets/index.html>`_.

Below is an example using :code:`TextBox`, :code:`Slider` and :code:`Button` widgets.


.. code-block:: python

    import numpy as np
    import matplotlib.pyplot as plt
    from matplotlib.widgets import Button, Slider, TextBox
    plt.ion()

    class MyAnimatedFigure():
        def __init__(self):
            self.fig = plt.figure(figsize=(10, 6)) #, tight_layout=True)
            self.ax = self.fig.add_subplot(111)
            self.fig.subplots_adjust(bottom=0.2, left=0.2)
            self.ax_slider_amp = self.fig.add_axes([0.1, 0.25, 0.0225, 0.63])
            self.ax_text_freq = self.fig.add_axes([0.31, 0.05, 0.3, 0.075])
            self.ax_button_reset = self.fig.add_axes([0.81, 0.05, 0.1, 0.075])
            # data
            self.ax.set_xlabel("Time [s]")
            self.ax.set_ylabel("amplitude")
            self.ax.set_ylim([-11, 11])
            self.ax.set_xlim([0, 1])
            self.t = np.linspace(0, 1, 10000)
            self.freq_init = 10
            self.amplitude_init = 5
            self.amplitude = self.amplitude_init
            self.freq = self.freq_init # Hz
            self.line = self.ax.plot(self.t, np.zeros(len(self.t)))
            # Slider Amplitude
            self.amp_slider = Slider(
                ax=self.ax_slider_amp,
                label="Amplitude",
                valmin=0,
                valmax=10,
                valinit=self.amplitude_init,
                orientation="vertical"
            )
            self.amp_slider.on_changed(self.ampl_changed)
            self.amp_slider.set_val(self.amplitude_init)
            # Button Reset
            self.bt_reset = Button(self.ax_button_reset, "Reset")
            self.bt_reset.on_clicked(self.reset_clicked)
            # Input frequency
            self.freq_input = TextBox(self.ax_text_freq, "Evaluate", textalignment="center")
            self.freq_input.on_submit(self.submit_freq)
            self.freq_input.set_val(f"{self.freq_init}")  # Trigger `submit` with the initial string.

        
        def submit_freq(self, expression):
            self.freq = eval(expression)
            self.update()

        def ampl_changed(self, val):
            self.amplitude = val
            self.update()
        
        def reset_clicked(self, event):
            self.amp_slider.reset()
            self.freq_input.set_val(f"{self.freq_init}")

        def update(self):
            new_ydata = self.amplitude * np.sin(2 * np.pi * self.freq * self.t)
            self.line[0].set_ydata(new_ydata)

    fig = MyAnimatedFigure()


**Result:**

.. image:: /assets/plots/widgets/example.gif
   :width: 600pt


------------------------------------------------------------

**Sources**:

- TextBox: https://matplotlib.org/stable/gallery/widgets/textbox.html
- Slider: https://matplotlib.org/stable/gallery/widgets/slider_demo.html
- Button: https://matplotlib.org/stable/gallery/widgets/buttons.html