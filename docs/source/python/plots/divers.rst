Divers
======

List of Matplotlib colormaps: https://matplotlib.org/stable/tutorials/colors/colormaps.html


Place the legend at the side of the ax:

.. code-block:: python

    import matplotlib.pyplot as plt

    fig = plt.figure(figsize=(5, 3), tight_layout=True)
    ax1 = fig.add_subplot(111)
    ax1.grid()
    ax1.plot([0, 1, 2, 3, 4, 5, 6, 7], label="test 1")
    ax1.plot([3, 4, 5, 3, 2, 1, 1, 2], label="test 2")
    ax1.plot([2, 3, 4, 5, 6, 7, 6, 7], label="_nolegend_")
    ax1.legend(loc='center left', bbox_to_anchor=(1.04, 0.5))
    fig.savefig("legend_side.png")


.. image:: /assets/plots/divers/legend_side.png
    :height: 300pt


Add text in a plot:

.. code-block:: python

    import matplotlib.pyplot as plt

    fig = plt.figure(figsize=(6, 3), tight_layout=True)
    ax1 = fig.add_subplot(111)
    ax1.grid()
    ax1.axis([0, 10, 0, 5])
    ax1.text(2, 4, "TEST ONE", family="serif")
    ax1.text(2, 3, "TEST TWO", va="top", style="italic")
    ax1.text(2, 2, "TEST THREE", ha="right", fontweight="bold")
    ax1.text(2, 1, "TEST FOUR", ha="center", fontsize=16)
    ax1.text(6, 3, "TEST FIVE", rotation=90, color='r')
    ax1.text(8, 3, "TEST SIX", rotation=-60, backgroundcolor="yellow")
    ax1.text(8, 1, "Text that needs to be wrapped because it's too long. " * 3, ha="center", wrap=True)
    fig.savefig("text.png")

.. image:: /assets/plots/divers/text.png
    :height: 300pt


Example using the fill_between feature to plot a typical mean + std value curve:

.. code-block:: python

    import matplotlib.pyplot as plt
    import numpy as np

    x = np.arange(25)
    y_mean = -2 + 0.3 * x - 0.6 * x ** 2 + 0.03 * x ** 3
    y_std = 50 * np.random.rand(len(x))

    fig = plt.figure(figsize=(7, 4), tight_layout=True)
    ax1 = fig.add_subplot(111)
    ax1.grid()
    ax1.set_xlabel("x")
    ax1.set_ylabel("y")
    ax1.set_title("Fill Between example")
    ax1.set_ylim([-60, 100])
    ax1.plot(x, y_mean, linewidth=2.0, label="Mean")
    ax1.fill_between(x, y_mean - (y_std / 2), y_mean + (y_std / 2), alpha=.5, linewidth=0, label="+/- STD/2")
    ax1.fill_betweenx(np.arange(-60, 100), 0, 2 * np.sin(2 * np.pi * np.arange(-60, 100) / 60) + 10, color='g', alpha=.3, linewidth=0, label="Beginning")
    ax1.fill_betweenx([-60, 100], 15, 24, color='r', alpha=.3, linewidth=0, label="End")
    ax1.legend()
    fig.savefig("fill_between.png")


.. image:: /assets/plots/divers/fill_between.png
    :height: 300pt


------------------------------------------------------------

**Sources**:

- matplotlib.pyplot.text: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.text.html
- matplotlib.pyplot.fill_between: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.fill_between.html
- matplotlib.pyplot.fill_betweenx: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.fill_betweenx.html
- matplotlib.patches.Patch: https://matplotlib.org/stable/api/_as_gen/matplotlib.patches.Patch.html
