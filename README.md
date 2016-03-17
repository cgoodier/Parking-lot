# Parkify
## The Application

<img src="http://i.imgur.com/MKaZBnj.png" width="400"> 

### A world in constant motion

We live in a world where being able to do things faster has become a necessity to the point where even five minutes can feel like an eternity. This is related with the fact that finding a parking space is more difficult everyday due to the constant increase of traffic; therefore, it makes this a tedious process, taking longer than what the world around us demands. Having no way to be able to explore an entire parking lot, one begins to lose time by small amounts a day until it becomes a tremendous total when adding those minutes in a lifetime; however, gas is also wasted in the process.

### What is Parkify?

Parkify is an application that aims to solve this simple activity that takes one's precious time, so you are able to find a spot to park your car in faster an effective way than by searching around. It is a simple solution which manages to separate a parking lot in different zones and tells you how many spaces are free in each of this areas. Also, when Parkify gets to know you better, it will be able to priotize the closest empty parking zones near your destination. This way, looking for a parking spot can stop being a stressful chore and become an easy daily step.

### How Parkify is achieved

This application was built from the ground up by utilizing an Intel Edison Board and simulating a parking environment with the use of sensors to mark the entry and exit of cars in an area, a database stored in a web server as well as a communication with the application through the use of .json files. To put it simply, we have an application which communicates utilizing a ssh protocol to the Edison and to a database, whenever an entry or exit sensor is triggered, the the app will change accordingly.
## Hardware and software
### Intel Edison
The Intel Edison board will be utilizing a button to count a car occupying a new space, a touch sensor to represent a car leaving a free space, and an LCD screen which lists the number of free parking spots and changes color between green, yellow, and red depending on this number, with green being more than half parking spaces available, yellow being half or less spaces available, and red when there is no availability left; The updates can only be received from the sensors after an interval of 0.05 seconds. The whole development for the Edison can be found in this link.

 [First Functional](https://github.com/dtoledo23/development_card/blob/master/firstFunctional.py "GitHub Repository")

#### Image Recognition
We have implementated the use of a camera in order to be able to detect license plates and to identify the cars entering the parking lot area; however, this feature is still in progress. It will capture video constantly and will take a picture whenever a license plate enters its field of view and then it will save the image and restart the camera so it is ready to detect another plate. Though this implementation isn’t complete yet, a rough version of the code is found here:

[Camara Test](https://github.com/dtoledo23/development_card/blob/master/cameraTest.py "GitHub Repository")
### Web service
We also included within the Intel Edison, we will be using its IP address to host a web server which will be used to display the main webpage of Parkify made utilizing a Materialize template as well as giving you the option to create your own parking space for the use of the application. The css and necessary scripts for it to run correctly can be found here as well as the server:

 [Init.js](https://github.com/iotchallenge2016/development_card/blob/950f8b029e6447204472b0a2a5b5659d0c9766be/static/js/init.js "GitHub Repository")

[Main.css](https://github.com/iotchallenge2016/development_card/blob/950f8b029e6447204472b0a2a5b5659d0c9766be/static/css/main.css "GitHub Repository")

[Server](https://github.com/iotchallenge2016/development_card/blob/950f8b029e6447204472b0a2a5b5659d0c9766be/server.py "GitHub Repository")

The server will not only manage the webpage, but it will also be in charge of reading the initial state of the parking lot's areas of through a csv file through a loader py file, then we have a another py file that makes requests to the server and writes into the lcd of the Edison with the use of its sensors. Finally we have another file which states when a request is invalid.

[Invalid Request](https://github.com/iotchallenge2016/development_card/blob/950f8b029e6447204472b0a2a5b5659d0c9766be/invalid_request.py "GitHub Repository")

[Loader](https://github.com/iotchallenge2016/development_card/blob/950f8b029e6447204472b0a2a5b5659d0c9766be/loader.py "GitHub Repository")

[Requests to server](https://github.com/iotchallenge2016/Parking-lot/blob/55d001aade8c4c797b8b5044747d22ce86add645/requests_to_server.py "GitHub Repository")

## Resources
Python sensor libraries:

[Intel's Python example](https://github.com/intel-iot-devkit/upm/tree/master/examples/python "Intel's GitHub Repository")


git branches:

[Create a new branch with git and manage branches](https://github.com/Kunena/Kunena-Forum/wiki/Create-a-new-branch-with-git-and-manage-branches "Kunena's GitHub Repository")

[Git Branching Basic: Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging "Git Webpage")


IOT internet of things:

[Internet of Things 101](https://www.gitbook.com/book/theiotlearninginitiative/internetofthings101/details "Gitbook")

##__Parking Lot__

For the construction of this software, the Tecnologico de Monterrey Campus Gualadajara's parking lot was used as a prototype example. The parking lot in this campus consists in ten different areas with the following parking spaces:

Zone | Parking Spaces
---|:---:
Medicine | 412
Visitors | 237
Congress Center | 288
Residences | 230
Civil Engineering | 291
Engineering | 446
Media | 229
Library | 269
Cafeteria | 341 
Entrance | 270

The following image is the campus' parking lot divided by zones. 

![ParkingTec](http://i.imgur.com/L5xOqdS.png)

##__How does it works?__

###__Sensors__
With the use of sensors, our program is able to detect the movement in front of them. This with the purpose of detecting wheter a car is exiting or entering a certain zone of the parking lot. This way our main system is able to keep count of how many cars are currently in the parking lot; therefore, it will get the number of available spaces. By keeping the count, we are also able to classify zones' disponibility by colors. This way it is easier for the user to keep track of the empty spaces since the LCD brights in that certain color: Red is not available, Yellow is half available and Green is more than half available. This was made by setting a maximum and minimum of Flags and assigning a value to each color: 

```python 
#Limits
MAX = 20
MIN = 0

#Limit flags

RedFlag = 0
YellowFlag = 10

 # ------ LCD -------
        #Red = 252, 18, 33
        #Amarillo = 229, 220, 22
        #Verde = 46, 254, 67

    if(lugares <= RedFlag):
        myLcd.setColor(252, 18, 3)

    elif(lugares <= YellowFlag):
        myLcd.setColor(229, 220, 22)

    else:
        myLcd.setColor(46, 254, 67)

    messages = "Disponibles: " + str(lugares) + " "
    myLcd.setCursor(0,0)
    myLcd.write(messages)
    
```
###__Inside the program__
Like previously mentioned, we also created a file which states when a request is invalid. This way, for example, if the user does not uses the application by 'get' the program will display an invalid message to the user himself. This was done effectively by writting the next code in file [server.py](https://github.com/iotchallenge2016/development_card/blob/web-service/server.py "Github repository") :

```python
def api_sections():
	if request.method == 'GET':
		data = []
		for result in mongo.db[COLLECTION_TO_USE].find():
			data.append(result)
		return Response(dumps(data), mimetype='application/json')
	else:
		raise InvalidUsage('Unsupported Method', 501)

@app.route('/sections/<sectionId>', methods = ['GET'])
```

Afterwards, by creating a class inside [invalid_request.py](https://github.com/iotchallenge2016/development_card/blob/web-service/invalid_request.py "GitHub repository") and defining the needed information: 
```python
from flask import jsonify

class InvalidRequest(Exception):
	status_code = None
	message = None
	payload = None

	def __init__(self, message, status_code, payload=None):
		Exception.__init__(self)
		self.message = message
		self.status_code = status_code
		self.payload = payload

	def to_dic(self):
		rv = dict(self.payload or ())
		rv['message'] = self.message
		return rv
```

We also made [graph.py](https://github.com/iotchallenge2016/development_card/blob/web-service/graph.py "GitHub Repository") in order to make the program possible to identify the structure of the parking lot. This way it localizes the entrances, sections, closest parking section to the user as well for the sections surrounding that section. This was by creating a Class named 'Graph' and defining the functions needed inside that class, for example, this is how we managed to make the program find the closest parking zone to the user in case he or she is in a hurry:
```python
def get_closest_parking_section(self, dstNodeId, tolerance=5):
		paths = []
		for i in self.find_entrances():
			path = nx.dijkstra_path(self.g, i, dstNodeId)
			while (self.g.node[path[-1]]['type'].upper() != 'PARKING'):
				path.pop()
			paths.append(path)

		destinations = []
		for i in xrange(0, len(paths)):
			destinations.append(paths[i][-1])

		for i in xrange(0,len(destinations)):
			section = self.g.node[destinations[i]]['section']
			free = float(section['capacity']) / section['max'] * 100
			prevFound = [destinations[i]]
			while (free < tolerance):
				destinations[i] = self.find_neighbor_with_parking_spots(destinations[i], exclude=prevFound)
				prevFound.append(destinations[i])
				section = self.g.node[destinations[i]]['section']
				free = float(section['capacity']) / section['max'] * 100

		if len(destinations) == 1:
			destinations = destinations[0]

		return destinations
```

###__Algorithm__
<p>In order to give the best performance to Parkify App, we connected each point of the campus and divided them in parking lot or buildings. This way the program search for empty spaces in the closest parking area to the user's destination inside the campus. For example, if the user is going to the Congress Center inside the campus, Parkify will beging by searching the availability inside such parking area. This way the user knows faster that there is or there is not an empty spot near to his destination. Therefore, if the parking area is full, Parkify will suggest the second closest area. For a better explanation of the algorithm please look foward to the next image: </p>
_Green: Parking Area_ | _Yellow = Building_ | _Blue = Entrance_
<p> <img src="https://github.com/iotchallenge2016/Parking-lot/blob/master/Graph.png?raw=true" width="400"> </p>
[Click here](https://github.com/iotchallenge2016/Parking-lot/tree/Algorith-desing "GitHub Repository") for the breakdown of this same algorithm.
