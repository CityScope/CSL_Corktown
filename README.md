# CSL_Corktown
Welcome to the Corktown CityScope project. This is an instance of the CityScope platform. More information about CityScope can be found here:

[CityScope](https://github.com/CityScope/cityscope.github.io)

This repository contains instructions for downloading and running the different components of the CityScope Corktown project. It is not necessary to clone this repository.

The Corktown CityScope project consists of 3 modules:
1. the mobility simulation module: [link](https://github.com/CityScope/CS_Mobility_Service)
2. the urban indicator module: [link](https://github.com/CityScope/CS_Urban_Indicators)
3. the interactive front-end tool: [link](https://github.com/CityScope/CS_cityscopeJS)

The functionality of each module is documented in the respective README files of the repositories. These modules interact with each other through the [city_IO](https://github.com/CityScope/CS_CityIO) API


To immediately start working with the live version of the tool, go to <https://cityscope.media.mit.edu/CS_cityscopeJS/?cityscope=corktown>. This tool allows open-ended scenario creation, exploration, saving, loading and downloading of results. For each scenario, the backend modules run simulations and compute urban indicators and these results are visualised in the tool. Note that this hosted version may not be maintained indefinitely. Ideally, you should run your own version of the software by following the instructions below. This will also allow you to make local changes to any of the three modules in order to enhance or alter the functionality of the tool. Using pull requests, such changes can also be shared with the parent repositories to enhance other CityScope projects.

![CityScope Corktown Screenshot](https://github.com/CityScope/CS_cityscopeJS/blob/master/docs/web_ui.jpg)


# Running CityScope Corktown locally

## Clone the projects and install dependencies

Follw the instructions below to clone the 3 modules and install their dependencies:

### Interactive Front-end
To clone the interactive Front-End module and checkout the right commit, navigate to the directory where you want to store the project and run the following commands:
```
> git clone https://github.com/CityScope/CS_cityscopeJS.git
> cd CS_cityscopeJS
> git checkout ce3a074
```

Ensure you have [Node.js](https://nodejs.org/en/) installed. In the project directory, you can run: `npm start` Runs the app in the development mode.<br /> Open [http://localhost:3000/?cityscope=corktown_dev](http://localhost:3000/?cityscope=corktown_dev) to view the tool in the browser. The tool will only update in response to new scenarios when the two back-end modules are running.

### Mobility Simulation Module
To clone the mobility simulation module and checkout the right commit, navigate to the directory where you want to store the project and run the following commands:
```
> git clone https://github.com/CityScope/CS_Mobility_Service.git
> cd CS_Mobility_Service
> git checkout bb4648a
```
Ensure that your version of Python is >=3.8. This project depends on some libraries which are listed in the requirements.txt file. It is recommended that you create a clean environment for this project before installing the dependencies. You can install the dependencies one by one using pip3 or by running the following command inside the project folder.
```
> pip3 install -r requirements.txt
```
### Urban Indicators Module
To clone the urban indicators module and checkout the right commit, navigate to the directory where you want to store the project and run the following commands:
```
> git clone https://github.com/CityScope/CS_Urban_Indicators.git
> cd CS_Urban_Indicators
> git checkout 7a5b55e
```

Ensure that your version of Python is >=3.8. This project also depends on some libraries which can be installed the same way as described above. 

### Dockerization
If there are any issues with library dependencies for the 2 Python modules, it may be better to use Docker. A docker image can be used to create a container for each module which already has all the necessary libraries installed. A Docker image has been created for these projects with all the necessary dependencies (see [urban-indicator-dev](https://github.com/crisjf/urban-indicator-dev)).

After the [Docker](https://www.docker.com/) software has been installed, pull the image
```
> docker pull crisjf/urban-indicators-dev
```

The following instructions are for running the Urban Indicators module in a Docker container. The same process can be followed to run the Mobility Simulation module in a container if necessary.
Run a container using the image. It will automatically start a bash.
The following command also makes sure that the current directory is mounted to the container (run from the directory of the repo):
```
> docker run -it -v "$(pwd)":/home/CS_Urban_Indicators crisjf/urban-indicators-dev
```

After cloning the project and installing the requirements, there are two ways you can work with the models: by running scripts to evaluate static scenarios or by using the interactive tool for open-ended exploration of scenarios.

## Running static scenarios 

### Mobility Simulation
The file CS_Mobility_Service/scripts/corktown_scenarios.py gives an example of how to evaluate the mobility simulation on static scenarios. This outputs a csv file containing for each scenario:
- values of the mobility indicators
- modal splits

To run this file, navigate to 'CS_Mobility_Service/scripts' and run:
```
python3 corktown_scenarios.py

```
See the [README](https://github.com/CityScope/CS_Mobility_Service) file for information on how to update the mobility parameters. The land-use scenarios are read directly from the stored scenarios on cityIO (created using the interactive tool). Any new scenario created using the interactive tool will be evaluated when this file runs. The results for each scenario will be saved as a csv.

### Urban Indicators
To evaluate the rest of the indicators (Innovation Potential, Economic, Sustaiable Buildings and Community Benefits) for the saved land-use scenarios, navigate to the CS_Urban_Indicators folder and run:

```
python3 corktown_offline_indicators.py
```

This outputs a csv file containing the values of all the individual indicators (raw and normalised) and the composite indicators, for each scenario.

## Running the Interactive Tool

In order to use the interactive tool locally, you will need to run three modules: the front-end, the mobility simulation module and the urban indicators module.

### Run the front-end
The CityScope front-end is a Javascript React project which runs in the web browser. From the project directory, `npm start` runs the app in the development mode.<br /> Open [http://localhost:3000/?cityscope=corktown_dev](http://localhost:3000/?cityscope=corktown_dev) to view the tool in the browser. The application will open in your browser when it finishes building.

### Run the Urban Indicators module
Navigate to the CS_Urban_Indicators folder and run:
```
python3 listen.py
```

### Run the Mobility Simulation Module
Navigate to the CS_Mobility_Service/scripts folder and run:
```
python3 corktown_listen.py

```

When all three modules are running, you can open your browser window and begin interacting with the tool.
