# Parkify
## The Application

<img src="http://i.imgur.com/MKaZBnj.png" width="400"> 

>"Welcome to the world of Parkify. I am Eagfy, and I will be your guide!.
<img src="http://i.imgur.com/dwoUlWR.jpg" width="400">

<p>Check out our promo video by clicking the image below!</p>
[![Parkify Video](https://i.ytimg.com/vi/xLYKoWhppAw/hqdefault.jpg)](https://www.youtube.com/watch?v=xLYKoWhppAw "Youtube video")

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
We have implemented the use of a camera in order to be able to detect license plates and to identify the cars entering the parking lot area; however, this feature is still in development. It will capture video constantly and will take a picture whenever a license plate enters its field of view and then it will save the image and restart the camera so it is ready to detect another plate. Though this implementation isn’t complete yet, a rough version of the code is found here:

[Camara Test](https://github.com/dtoledo23/development_card/blob/master/cameraTest.py "GitHub Repository")
### Web service
We also included within the Intel Edison, we will be using its IP address to host a web server which will be used to display the main webpage of Parkify made utilizing a Materialize template as well as giving you the option to create your own parking space for the use of the application. The HTML, CSS, and necessary scripts for it to run correctly can be found here as well as the server:

 [HTML](https://github.com/iotchallenge2016/development_card/tree/web-service/templates "GitHub Repository")

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
Through the use of sensors, our program is able to detect the movement in front of them. This with the purpose of detecting wether a car is exiting or entering a certain zone of the parking lot. Thus our main system is able to keep count of how many cars are currently in the parking lot and use that to calculate the number of free spaces. By keeping the count, we are also able to classify zones' disponibility by colors, making it easier for the user to keep track of the empty spaces through the use of different hues in an LCD screen: Red is not available, Yellow is half available and Green is more than half available. This was made by setting a maximum and minimum of Flags and assigning a value to each color: 

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
As it was previously mentioned, a file that states when a request is invalid was created. With the purpose that if, for example, the user does not uses the application by 'get' the program will display an invalid message to the user himself. This was done effectively by writting the next code in file [server.py](https://github.com/iotchallenge2016/development_card/blob/web-service/server.py "Github repository") :

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

Afterwards, by creating a class inside [invalid_request.py](https://github.com/iotchallenge2016/development_card/blob/web-service/invalid_request.py "GitHub repository") and defining the needed information the request is set as invalid: 
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

The file [graph.py](https://github.com/iotchallenge2016/development_card/blob/web-service/graph.py "GitHub Repository") was also made in order to make it possible for the program to identify the structure of the parking lot. Hence it localizes the entrances, sections, closest parking section to the user as well for the sections surrounding that section. This was by creating a Class named 'Graph' and defining the functions needed inside that class, for example, this is how we managed to make the program find the closest parking zone to the user in case he or she is in a hurry:
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
[Click here](https://github.com/iotchallenge2016/Parking-lot/blob/Algorith-desing/README.md "GitHub Repository") for the breakdown of this same algorithm.

###__The App__
For the development of the app, we created a simple but fast design. This way the user does not wastes several time and concentration trying to read the app. The app, mainly, asks for the specific user's destination. This way the app returns the information of that area in order to show if that zone is available or not. We made this possible by extracting the information from the database and projecting the same information to the user. This is an example of how we did it inside file [JASONAdapter.java](https://github.com/iotchallenge2016/android_app/blob/master/app/src/main/java/com/equipo7/iot2016/JSONAdapter.java) :
```java
public View getView(int position, View convertView, ViewGroup parent) {
        if(convertView == null){
            convertView = activity.getLayoutInflater().inflate(R.layout.json_row, null);
        }

        TextView section = (TextView)convertView.findViewById(R.id.textView);
        TextView lugares = (TextView)convertView.findViewById(R.id.textView2);
        TextView per = (TextView)convertView.findViewById(R.id.textView3);
        LinearLayout layout = (LinearLayout)convertView.findViewById(R.id.layout);

        try{
            String sec = this.json.getString("section") + "";
            sec = sec.replace("P_", "");
            section.setText("Estacionamiento " + sec);

            lugares.setText(new Double(this.json.getString("capacity")).intValue() + "");

            Integer percentage = new Double((Double.parseDouble(this.json.getString("capacity")) * 100) / Double.parseDouble(this.json.getString("max"))).intValue();
```
<p>Like previously mentioned we made it simple, so the does not gets distracted. This is because the user is driving and if the app is hard to read it could cause harm to the user. We also complemented this with colors and percentage since looking to a green image is easier for the user to know that the parking area near his destination is availabl. Open, search, quick look, go; all of this in less than 10 seconds. </p>

![Ejemplo App](http://i.imgur.com/zXKYnSC.png)

###__The Web page__
[Check out our Web page!](http://10.43.51.167:5000/ "Parkify Web")

A web page for the application was made so that those who are interested in how their parking lot is being used can see it on a visual represantion of a map. It was made utilizing the front-end framework materilize to give the user a better experience. It is divided in four sections: Home, View, About Us, and Documentation.

On the home page the user is prompted to upload their parking lot through a .csv file containing their initial values for how each of the parking zones are filled at the moment, reading each row in the document and identyfiyng the zone and the number of occupied and free spots in each of them. This is done with the following code in [loader.py](https://github.com/iotchallenge2016/development_card/blob/web-service/loader.py "GitHub Repository"):

```python
def parse_csv(csvURL):
    avlb = 2
    zone = 0
    values = []
    total = 0
    diff = ""
    json = "["
    with open(csvURL, 'r') as csv:
        for line in csv.readlines():
            elements = line.strip().split(',')
            if diff == "":
                diff = (elements[zone])
            elif diff != elements[zone]:
                ocpd = sum(values)
                free = total - ocpd
                json = json + toJSON(diff,free, total)
                values = []
                total = 0
                diff = elements[zone]
            values.append(int(elements[avlb]))
            total += 1
    ocpd = sum(values)
    free = total - ocpd
    return json + toJSONfinal(diff,free, total) + "]"

def toJSON(zone,available,total):
    return "{\"section\":\""+zone+"\", \"capacity\":"+str(available)+",\"max\":"+str(total)+"},"

def toJSONfinal(zone,available,total):
    return "{\"section\":\""+zone+"\", \"capacity\":"+str(available)+",\"max\":"+str(total)+"}"
```

In the page _View_, a map is initialized utilizing Google Maps where the user is able to see how the different zones of the parking lot are filled; different colors are used according to how many spaces are occupied in a zone, starting from dark red (full lot) and continuing into lighter tones of blue (empty lot). Besides the map, we can find cards, or blocks, that share the same colors as the different areas of the map and give the number of free place and percentage of how many spots occupied in the parking lot with updates every 5 seconds. Both of this things can be found in  [main.js](https://github.com/iotchallenge2016/development_card/blob/web-service/static/js/main.js "GitHub Repository").

```javascript
var polygons = [];

function initMap() {
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 17,
    center: {lat: 20.734485, lng: -103.454752},
    mapTypeId: google.maps.MapTypeId.TERRAIN,
    scrollwheel: true,
    draggable: true
  });

  var zones = [];
  var percentages = [];
  $.getJSON('/sections', function(data) {
  	for (var i = data.length - 1; i >= 0; i--) {
  		zones.push(data[i].location);
  		percentages.push(Math.floor((data[i].max - data[i].capacity) / data[i].max * 100));
  	}
    for (var i = zones.length - 1; i >= 0; i--) {
      var zone = new google.maps.Polygon({
        paths: zones[i],
        strokeColor: getColorClass(percentages[i])[1],
        strokeOpacity: 0.8,
        strokeWeight: 2,
        fillColor: getColorClass(percentages[i])[1],
        fillOpacity: 0.35
      });
      polygons.push(zone);
      zone.setMap(map);
    }  	
  })

}

function getColorClass(percentage) {
	
	if (percentage > 90) {
		return ["red accent-4", "#d50000"];
	} else if(percentage > 80) {
		return ["red", "#f44336"];
	} else if(percentage > 70) {
		return ["orange", "#ff9800"];
	} else if(percentage > 60) {
		return ["amber darken-3", "#ff8f00"];
	} else if(percentage > 40) {
		return ["yellow accent-2", "#ffff00"];
	} else if(percentage > 20) {
		return ["lime accent-4", "#aeea00"];
	} else {
		return ["light-green accent-3", "#76ff03"];
	}
}

function loadCards() {
  $.getJSON('/sections', function(data) {
    var html = "";
    for (var i = data.length - 1; i >= 0; i--) {
      html += "<div class='col s12'>";
      var percentage = Math.floor((data[i].max - data[i].capacity) / data[i].max * 100);
    	html += "<div class='card "+ getColorClass(percentage)[0] +"'>";
    	html += "<div class='card-content white-text'>";
    	html += "<div class='card-title'>" + data[i].section.replace("P_", "Estacionamiento ") + "</div>";
    	html += "<p>Lugares Disponibles: " + (data[i].max - (data[i].max - data[i].capacity)).toString() + "</p>";
    	html += "<p>Ocupación: " + percentage + "%</p>";
    	html += "</div>";
    	html += "</div>";
    	html += "</div>";
      if (polygons.length > 0) {
        polygons[i].setOptions({
          strokeColor: getColorClass(percentage)[1],
          fillColor: getColorClass(percentage)[1]
        });
      }
		}
		console.log('Refreshing')
		$('#cards-container').html(html);
	});
	setTimeout(loadCards, 2000);
}

loadCards();
```
<p>In the next section _About Us_ we have a short description of what Parkify is as well as what our objective is, followed by all the members in the team. Also in this page, a short video is included, where we present the way the problem parkify is trying to solve, the solution that is being offered offering, and a short description of how the app works. The last section, _Documentation_, holds a list of all the available commands that can be given to the application and what each of these is for, making it easy of the user to navigate and work in a way he sees fit. This is a screenshot of our webpage: </p>

![ScreenshotWeb](http://i.imgur.com/T0qQJz4.png)
###__Model__

To show how parkify works, a miniature model of a parking lot was made out of styrofoam. Here we use two different areas that are connected in the middle, each 20 spaces. An Intel Edison board was placed below the two zonees adding a button at their entrance and a touch sensor in their exit as well as an LCD screen just before their respective buttons to display the number of free spaces and change color according to them (as it was mentioned in the _Sensors_ section) and a parking gate next to the entrance and exit of the parking lot that uses a servo motor to move every five seconds.

The way the model works: A car enters the parking lot and passes through the button at the entrance, decreasing the counter of the first zone. It will either stay in the first area or go to the second one according to where the algorith tells it to go; if it stays in the first area then it will just park there, otherwise it will move on to the second one passing in its way to through the touch sensor in the first exit, increasing the "free" counter in that area, an then passing through the button of the second area to change its counter respectively and park. When the car needs to exit, if its from the second area, then he just passes through the touch sensor and the gate at the exit, if it's located in the first, then it will first have to pass through the second area.

<img src="http://i.imgur.com/fvu9Bp7.png" width="400">

## TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION (GPLv2)

0. This License applies to any program or other work which contains a notice placed by the copyright holder saying it may be distributed under the terms of this General Public License. The "Program", below, refers to any such program or work, and a "work based on the Program" means either the Program or any derivative work under copyright law: that is to say, a work containing the Program or a portion of it, either verbatim or with modifications and/or translated into another language. (Hereinafter, translation is included without limitation in the term "modification".) Each licensee is addressed as "you".

Activities other than copying, distribution and modification are not covered by this License; they are outside its scope. The act of running the Program is not restricted, and the output from the Program is covered only if its contents constitute a work based on the Program (independent of having been made by running the Program). Whether that is true depends on what the Program does.

1. You may copy and distribute verbatim copies of the Program's source code as you receive it, in any medium, provided that you conspicuously and appropriately publish on each copy an appropriate copyright notice and disclaimer of warranty; keep intact all the notices that refer to this License and to the absence of any warranty; and give any other recipients of the Program a copy of this License along with the Program.

You may charge a fee for the physical act of transferring a copy, and you may at your option offer warranty protection in exchange for a fee.

2. You may modify your copy or copies of the Program or any portion of it, thus forming a work based on the Program, and copy and distribute such modifications or work under the terms of Section 1 above, provided that you also meet all of these conditions:

    a) You must cause the modified files to carry prominent notices stating that you changed the files and the date of any change. 
    b) You must cause any work that you distribute or publish, that in whole or in part contains or is derived from the Program or any part thereof, to be licensed as a whole at no charge to all third parties under the terms of this License. 
    c) If the modified program normally reads commands interactively when run, you must cause it, when started running for such interactive use in the most ordinary way, to print or display an announcement including an appropriate copyright notice and a notice that there is no warranty (or else, saying that you provide a warranty) and that users may redistribute the program under these conditions, and telling the user how to view a copy of this License. (Exception: if the Program itself is interactive but does not normally print such an announcement, your work based on the Program is not required to print an announcement.) 

These requirements apply to the modified work as a whole. If identifiable sections of that work are not derived from the Program, and can be reasonably considered independent and separate works in themselves, then this License, and its terms, do not apply to those sections when you distribute them as separate works. But when you distribute the same sections as part of a whole which is a work based on the Program, the distribution of the whole must be on the terms of this License, whose permissions for other licensees extend to the entire whole, and thus to each and every part regardless of who wrote it.

Thus, it is not the intent of this section to claim rights or contest your rights to work written entirely by you; rather, the intent is to exercise the right to control the distribution of derivative or collective works based on the Program.

In addition, mere aggregation of another work not based on the Program with the Program (or with a work based on the Program) on a volume of a storage or distribution medium does not bring the other work under the scope of this License.

3. You may copy and distribute the Program (or a work based on it, under Section 2) in object code or executable form under the terms of Sections 1 and 2 above provided that you also do one of the following:

    a) Accompany it with the complete corresponding machine-readable source code, which must be distributed under the terms of Sections 1 and 2 above on a medium customarily used for software interchange; or, 
    b) Accompany it with a written offer, valid for at least three years, to give any third party, for a charge no more than your cost of physically performing source distribution, a complete machine-readable copy of the corresponding source code, to be distributed under the terms of Sections 1 and 2 above on a medium customarily used for software interchange; or, 
    c) Accompany it with the information you received as to the offer to distribute corresponding source code. (This alternative is allowed only for noncommercial distribution and only if you received the program in object code or executable form with such an offer, in accord with Subsection b above.) 

The source code for a work means the preferred form of the work for making modifications to it. For an executable work, complete source code means all the source code for all modules it contains, plus any associated interface definition files, plus the scripts used to control compilation and installation of the executable. However, as a special exception, the source code distributed need not include anything that is normally distributed (in either source or binary form) with the major components (compiler, kernel, and so on) of the operating system on which the executable runs, unless that component itself accompanies the executable.

If distribution of executable or object code is made by offering access to copy from a designated place, then offering equivalent access to copy the source code from the same place counts as distribution of the source code, even though third parties are not compelled to copy the source along with the object code.

4. You may not copy, modify, sublicense, or distribute the Program except as expressly provided under this License. Any attempt otherwise to copy, modify, sublicense or distribute the Program is void, and will automatically terminate your rights under this License. However, parties who have received copies, or rights, from you under this License will not have their licenses terminated so long as such parties remain in full compliance.

5. You are not required to accept this License, since you have not signed it. However, nothing else grants you permission to modify or distribute the Program or its derivative works. These actions are prohibited by law if you do not accept this License. Therefore, by modifying or distributing the Program (or any work based on the Program), you indicate your acceptance of this License to do so, and all its terms and conditions for copying, distributing or modifying the Program or works based on it.

6. Each time you redistribute the Program (or any work based on the Program), the recipient automatically receives a license from the original licensor to copy, distribute or modify the Program subject to these terms and conditions. You may not impose any further restrictions on the recipients' exercise of the rights granted herein. You are not responsible for enforcing compliance by third parties to this License.

7. If, as a consequence of a court judgment or allegation of patent infringement or for any other reason (not limited to patent issues), conditions are imposed on you (whether by court order, agreement or otherwise) that contradict the conditions of this License, they do not excuse you from the conditions of this License. If you cannot distribute so as to satisfy simultaneously your obligations under this License and any other pertinent obligations, then as a consequence you may not distribute the Program at all. For example, if a patent license would not permit royalty-free redistribution of the Program by all those who receive copies directly or indirectly through you, then the only way you could satisfy both it and this License would be to refrain entirely from distribution of the Program.

If any portion of this section is held invalid or unenforceable under any particular circumstance, the balance of the section is intended to apply and the section as a whole is intended to apply in other circumstances.

It is not the purpose of this section to induce you to infringe any patents or other property right claims or to contest validity of any such claims; this section has the sole purpose of protecting the integrity of the free software distribution system, which is implemented by public license practices. Many people have made generous contributions to the wide range of software distributed through that system in reliance on consistent application of that system; it is up to the author/donor to decide if he or she is willing to distribute software through any other system and a licensee cannot impose that choice.

This section is intended to make thoroughly clear what is believed to be a consequence of the rest of this License.

8. If the distribution and/or use of the Program is restricted in certain countries either by patents or by copyrighted interfaces, the original copyright holder who places the Program under this License may add an explicit geographical distribution limitation excluding those countries, so that distribution is permitted only in or among countries not thus excluded. In such case, this License incorporates the limitation as if written in the body of this License.

9. The Free Software Foundation may publish revised and/or new versions of the General Public License from time to time. Such new versions will be similar in spirit to the present version, but may differ in detail to address new problems or concerns.

Each version is given a distinguishing version number. If the Program specifies a version number of this License which applies to it and "any later version", you have the option of following the terms and conditions either of that version or of any later version published by the Free Software Foundation. If the Program does not specify a version number of this License, you may choose any version ever published by the Free Software Foundation.

10. If you wish to incorporate parts of the Program into other free programs whose distribution conditions are different, write to the author to ask for permission. For software which is copyrighted by the Free Software Foundation, write to the Free Software Foundation; we sometimes make exceptions for this. Our decision will be guided by the two goals of preserving the free status of all derivatives of our free software and of promoting the sharing and reuse of software generally.

##NO WARRANTY

11. BECAUSE THE PROGRAM IS LICENSED FREE OF CHARGE, THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW. EXCEPT WHEN OTHERWISE STATED IN WRITING THE COPYRIGHT HOLDERS AND/OR OTHER PARTIES PROVIDE THE PROGRAM "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING, REPAIR OR CORRECTION.

12. IN NO EVENT UNLESS REQUIRED BY APPLICABLE LAW OR AGREED TO IN WRITING WILL ANY COPYRIGHT HOLDER, OR ANY OTHER PARTY WHO MAY MODIFY AND/OR REDISTRIBUTE THE PROGRAM AS PERMITTED ABOVE, BE LIABLE TO YOU FOR DAMAGES, INCLUDING ANY GENERAL, SPECIAL, INCIDENTAL OR CONSEQUENTIAL DAMAGES ARISING OUT OF THE USE OR INABILITY TO USE THE PROGRAM (INCLUDING BUT NOT LIMITED TO LOSS OF DATA OR DATA BEING RENDERED INACCURATE OR LOSSES SUSTAINED BY YOU OR THIRD PARTIES OR A FAILURE OF THE PROGRAM TO OPERATE WITH ANY OTHER PROGRAMS), EVEN IF SUCH HOLDER OR OTHER PARTY HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.

### END OF TERMS AND CONDITIONS
