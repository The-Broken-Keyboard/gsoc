# GSoC-2025 

This is an overview of the work that I did over the last 12 weeks as part of Google Summer of Code 2025 for [ArviZ](https://python.arviz.org/en/stable/), under the NumFOCUS umbrella organization. Links to pushed code/relevant PRs and illustrations are included below and can be checked for more details.

## Project Overview: ArviZ Plots Feature Parity

ArviZ is in the process of being modularized into separate subpackages- [arviz-base](https://github.com/arviz-devs/arviz-base), [arviz-stats](https://github.com/arviz-devs/arviz-stats), and [arviz-plots](https://github.com/arviz-devs/arviz-plots)-with the goal of making the library more maintainable and extensible. My GSoC project focused on contributing to `arviz-plots`, the dedicated module for visualization, by:
- Migrating and enhancing key legacy plotting functions (e.g., [pair_plot](https://github.com/arviz-devs/arviz-plots/blob/main/src/arviz_plots/plots/pair_plot.py), [parallel_plot](https://github.com/arviz-devs/arviz-plots/blob/main/src/arviz_plots/plots/parallel_plot.py)).
- Implementing **new visualizations**, such as [pair_focus_plot](https://github.com/arviz-devs/arviz-plots/blob/main/src/arviz_plots/plots/pair_focus_plot.py)
- Introducing a fully functional **dark theme** ([arviz-tumma](https://github.com/arviz-devs/arviz-plots/blob/main/src/arviz_plots/styles/arviz-tumma.mplstyle)).
- Enhancing existing utilities like [dist_plot](https://github.com/arviz-devs/arviz-plots/blob/main/src/arviz_plots/plots/dist_plot.py) with **filled faces** and **histogram refinements**.

The work required deep engagement with ArviZ’s faceting managers ([PlotCollection](https://github.com/arviz-devs/arviz-plots/blob/main/src/arviz_plots/plot_collection.py) and [PlotMatrix](https://github.com/arviz-devs/arviz-plots/blob/main/src/arviz_plots/plot_matrix.py)), data handling with `xarray.DataTree`, and ensuring consistent design patterns across plotting functions.

## My Work

During the bonding period (May 8 – June 2), after reviewing differences between `arviz-plots` and `arviz`, we listed down several features which were missing in `arviz-plots` and several new features which were required. Following discussions with my mentors, we redefined and finalized a new prioritized list of tasks for the coding phase. Our aim to complete tasks as many as possible from the list during the coding phase.

1. [Initial Pair Focus Plot](https://github.com/arviz-devs/arviz-plots/pull/276)
- Implemented initial version of new `pair_focus_plot` function for exploring relationships between a focus variable and several others.

2. [More improvements in Pair Focus Plot](https://github.com/arviz-devs/arviz-plots/pull/282)
- Improved `pair_focus_plot` function to support `focus_var` of `dataarray` type.
- Improved logic for `focus_var` data extraction and label handling.
- Added support for divergence highlighting, axis/label sharing, and documentation/examples.

3. [Pair Plot](https://github.com/arviz-devs/arviz-plots/pull/287)
- Migrated and extended `pair_plot` to arviz-plots using the `PlotMatrix` manager.
- It also required new methods in `PlotMatrix` to map a particular row or column in the matrix. Therefore, I added `map_row` and `map_col` methods to `PlotMatrix`.
- Implemented triangle modes (`lower`, `upper`, `both`) with diagonal marginals powered by `plot_dist`.
- Solved divergence mask handling within the `PlotMatrix.map()`.

4. [Parallel Plot](https://github.com/arviz-devs/arviz-plots/pull/300)

- Added parallel_plot for visualizing high-dimensional samples across variables.
- Designed a stacking-based restructuring of sampling dimensions into a unified `sample` dimension.
- Supported divergence highlighting, xtick label rotation, and dynamic figure resizing to prevent compression.

5. [Enhancements to Dist Plot](https://github.com/arviz-devs/arviz-plots/pull/305)

- Introduced filled distributions (`KDE`, `histogram`, `ECDF` with `shaded faces`).
- Refactored `step` vs. `filled` histogram handling into dedicated backend functions for API consistency.
- Added tests for both `filled` and `step` histogram variants.

6. [Tumma: The Dark Theme](https://github.com/arviz-devs/arviz-plots/pull/309)

- Designed and implemented the **arviz-tumma** dark theme.
- Unified color cycles for light (**arviz-variat**) and dark theme(**arviz-tumma**) (reduced to 8 accessible, color-blind friendly colors).
- Replaced all hardcoded colors with dynamically computed ones using `YIQ` brightness contrast utilities.
- Extended support across `Matplotlib`, `Bokeh`, and `Plotly` backends.

7. [Plot LM](https://github.com/arviz-devs/arviz-plots/pull/328)

- Implemented **plot_lm** to visualize `regression-like datasets` with:
    - Multiple CI bands (e.g., 50% + 90%).
    - Mean or median lines.
    - Scatter plots of observed data.
- Supported multiple independent variables and multiple CI bands handling.
- Extended functionality to work seamlessly with both observed and predictive data.

### Other works 
- Improved documentation using Sphinx with multiple gallery-ready examples.
- Refined and extended test coverage across new features using `pytest` and `hypothesis`.
- Contributed to `arviz-base` too, to enable `labeller` selection in function `dataset_to_dataarray`. [(PR Link)](https://github.com/arviz-devs/arviz-base/pull/81)

## Current Status

Following are the primary PRs I have worked on:

1. Pair Focus Plot [[276](https://github.com/arviz-devs/arviz-plots/pull/276) & [282](https://github.com/arviz-devs/arviz-plots/pull/282)] **(Merged)**
2. Pair Plot [[287](https://github.com/arviz-devs/arviz-plots/pull/287)] **(Merged)**
3. Parallel Plot [[300](https://github.com/arviz-devs/arviz-plots/pull/300)] **(Merged)**
4. Dist Plot Enhancement [[305](https://github.com/arviz-devs/arviz-plots/pull/305)] **(Merged)**
5. Dark theme [[309](https://github.com/arviz-devs/arviz-plots/pull/309)] **(Merged)**
6. Plot LM [[328](https://github.com/arviz-devs/arviz-plots/pull/328)] **(In Progress)**

Other side works:
- Labeller selection enhancement [[81](https://github.com/arviz-devs/arviz-base/pull/81)] **(Merged)**


## Work left

- `Circular Variable` support in `plot_dist`
- `Dot plot` support.
- Extensive tests for backend functions. 

Since work was going on in `plot_dist`, so we decided to pause the work on `circular variable` and `dot plot`. These can be picked up later. Also tests are required for all the backend module functions.

## Challenges Faced, Learnings & Conclusion

This project was both technically and conceptually challenging. The most difficult parts included:

- **Understanding data reshaping in `xarray.DataTree` to support plots like `parallel_plot` and `plot_lm`**
  - A deep understanding and precise manipulation of the data structure were crucial to effectively use the plotting managers.
  - Different structures led to different ways of calling plotting functions, which could lead to completely different outputs or errors.  

- **Understanding plotting managers (`PlotCollection`, `PlotMatrix`), which served as the brain of the system**
  - Grasping the concepts of aesthetic mapping and visual elements was essential for effectively leveraging these managers.
  - Plotting managers handle complex aspects like aesthetic mapping and subplot arrangement, allowing individual plot functions to focus on rendering specific visual elements. This design provides fine-grained control over every visual element and its aesthetics, making it one of the most challenging yet important aspects to learn at the initial stages.  

- **API design and user experience**
  - Ensuring consistent behavior across multiple plotting functions was a significant challenge.
  - Consistent API was crucial for user experience, while dealing with complex plots and multiple backends.  

- **Visualization design and accessibility**
  - Choosing a color-blind–friendly color cycle that also met contrast requirements against dark backgrounds.  
  - Managing overlaying of elements in `plot_parallel`.  
  - Handling different types of labels and their rotations effectively.  

Through this experience, I learned:
- How to navigate and extend a large, modular codebase.
- Effective use of **Matplotlib**, **Plotly**, **Bokeh**, **Pytest**, **Hypothesis**, and **Sphinx**.
- Advanced data visualization design patterns, including **reusability** and **abstraction** with visual element functions.
- Best practices for contributing to open source:
    - writing **good documentation**, 
    - **good coding practices**, 
    - **designing for extensibility**, 
    - **collaborating effectively** with global mentors

Working with [Osvaldo Martin](https://github.com/aloctavodia), [Oriol Abril Pla](https://github.com/OriolAbril), and [Rohan Babbar](https://github.com/rohanbabbar04) has been an invaluable learning experience.

Overall, GSoC 2025 gave me the opportunity to make meaningful contributions to ArviZ, grow my technical and collaboration skills, and deepen my understanding of Bayesian visualization.

More detailed blog of my jouney in GSoC 2025 with ArviZ can be found [here](https://the-broken-keyboard.github.io/posts/gsoc/)


