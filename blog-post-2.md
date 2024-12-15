# Development Diary 15.12.2024

## Meet - The Web Application
The journey of a thousand miles begins with a single step, and our latest step is the web application! Distributing just a CLI tool for device management is challenging. Descriptions and technical documentation often fail to convey the full picture. Plus, not everyone is comfortable installing packages on their machines. Enter the web GUI!

While configuration setup remains a CLI-only feature for now, we've focused our web development efforts on enhancing user functionality for running and interacting with Docker containers.

Web App code is available in [this repo](https://github.com/Smehnov/rn-gui).

But let us introduce some of functionality:

### Settings
Everything starts with the settings. Here, you can specify the RPC node address, organization key, and user key. Simple and intuitive.
![Setings modal](https://i.ibb.co/N7bztzc/photo-2024-12-09-18-54-15.jpg)

### List robots
Need to see your fleet at a glance? The web app allows you to view a comprehensive list of all your robots.
![List robots](https://i.ibb.co/XYFxy2Y/photo-2024-12-09-18-49-12.jpg)

### Add jobs
Adding jobs has never been easier. Just like in the CLI, the web application lets you create new jobs, including example ones. It's as simple as filling out a form.
![Add Job](https://i.ibb.co/XSw5DVG/photo-2024-12-09-19-03-32.jpg)

### List jobs
Curious about what your robots are up to? The job list feature lets you see all running and completed jobs. While we're ironing out some date issues, the core functionality is solid.
![list jobs](https://i.ibb.co/kqczjz5/photo-2024-12-09-19-01-50.jpg)

### Job Info
Dive deep into the details with job information (even in CLI it is hottest feature of latest release) 
![](https://i.ibb.co/Smgm608/photo-2024-12-09-19-05-18.jpg)

### Web Terminal
Connecting remotely to a Docker terminal through your browser? Yes, please! We already did this with socket.io, but now full-fledged signed libp2p messages for enhanced security and consistency with agent's philosophy.
![Web Terminal](https://i.ibb.co/Hh2tFpF/photo-2024-12-09-19-08-07.jpg)

## Next Steps
1. Bug fixing, polishing, testing, repeating
2. Web App gona be deployed at any cost (via Github Pages)
3. Thinking hard about system of "topic widgets" for some data streams. Imagine running job with camera capture and stream it with libp2p in your browser.
