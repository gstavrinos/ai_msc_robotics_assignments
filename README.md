# ai_msc_robotics_assignments

Docker image and Assignments for the AI MSc Robotics Course

## Instructions

### Build

Change directory to the root of this repository. Execute:

`docker build -f docker/Dockerfile -t ai_msc_robotics_assignments .`

### Run

`docker run --rm --name ai_msc_robotics_assignments -p 9090:9090 -p 8888:8888 -p 8000:8000 -p 8080:8080 ai_msc_robotics_assignments`

### Stop/Kill

Press Ctrl+C 2-3 times to exit the container. Execute:

`docker kill ai_msc_robotics_assignments`

### Notes

- The work you do inside the container is not saved! Work externally on your editor of choice, and either copy and paste your code in the appropriate notebook block, or drag and drop your solution inside the jupyter lab and execute it on a separate terminal.
