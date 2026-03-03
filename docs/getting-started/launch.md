# Launch

<div style="text-align: center; margin-top: 2em;">
  <iframe width="640" height="360"
          src="https://www.youtube.com/embed/NV5SJhRC6q4?autoplay=1&loop=1&playlist=NV5SJhRC6q4&mute=1"
          frameborder="0"
          allow="autoplay"
          allowfullscreen>
  </iframe>
</div>

Now that the environment is ready, let's launch our first **nuRemics** application:

1. **Start the demo application.** <br>
From the root of the repository, use Pixi to start the application.
    ```bash
    pixi run DEMO_APP
    ```

2. **Access the interface.** <br>
Once started, the demo application will automatically open in your default web browser.

3. **Understanding the application context.** <br>
Take a moment to review the overall context of the application:
    - A central visual provides an overview of what the application is doing. Click on it to access a functional documentation of the scientific use case that is being addressed.
    - The top header displays the name of the application. Click on it to access the software-level documentation of the application.

4. **Set the working directory.** <br>
Specify the local path on the machine where the application data will be stored.

5. **Pre-configure the application.** <br>
To jumpstart the setup, pull pre-configured data directly from the **SplineCloud** platform. Click the _Configure from SplineCloud_ button to automatically populate the working directory with the studies and input datasets ready for execution:
    - Two distinct studies, `Study_Shape` and `Study_Velocity`, are declared.
    - `Study_Shape` is configured to explore variations of the `nb_sides` input parameter, while `Study_Velocity` focuses on variations of the `velocity.json` input path.
    - Each study includes three input datasets (`Test1`, `Test2`, `Test3`) to test different variations of the inputs.

6. **Run the application.** <br>
Click the _Run App_ button to start the computations. The application will automatically iterate through each study (`Study_Shape`, `Study_Velocity`) and its associated input datasets (`Test1`, `Test2`, `Test3`), producing the scientific results directly in the working directory. Once complete, the results can be visualized directly through the application interface.

---

<!-- <div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="../explore/" class="md-button md-button--primary">
    Explore
  </a>
</div> -->

<div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-top: 1.5rem;">
  <a href="https://www.suffisciens.com/labsvision"
     target="_blank"
     rel="noopener noreferrer"
     class="md-button md-button--primary">
    Onboarding
  </a>
</div>

---