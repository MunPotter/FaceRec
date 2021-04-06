                                                   
  <!--[![Backers on Open Collective](https://opencollective.com/nest/backers/badge.svg)](https://opencollective.com/nest#backer)
  [![Sponsors on Open Collective](https://opencollective.com/nest/sponsors/badge.svg)](https://opencollective.com/nest#sponsor)-->
## Face Recognition and detection


## Library
cmake
dlib
face_recognition
numpy
opencv-python
Flask
## Description

Although many face recognition opencv algorithms have been developed over the years, their speed and accuracy balance has not been quiet optimal . But some recent advancements have shown promise.Face recognition is a series of several problems.First, look at a picture and find all the faces in it Second, focus on each face and be able to understand that even if a face is turned in a weird direction or in bad lighting, it is still the same person.Third, be able to pick out unique features of the face that you can use to tell it apart from other people— like how big the eyes are, how long the face is, etc.Finally, compare the unique features of that face to all the people you already know to determine the person’s name.
Inthis project we use Flask. Flask is a micro web framework written in Python. It can create a REST API that allows you to send data, and receive a prediction as a response.

this is confidential project .here is some function

## Installation

```bash
import face_recognition
import cv2
import numpy as np
from flask import Flask
from flask import Flask, request, jsonify
import matplotlib
```

## Flask API

```bash
def post():
	if request.method == 'POST':
		print("got request")
		try:
			imagesave = request.files.get('image')

			if allowed_image(imagesave.filename):

				imagefile = imagesave.read()
				npimg = numpy.fromstring(imagefile, numpy.uint8)
				img = cv2.imdecode(npimg, cv2.IMREAD_UNCHANGED)

                #new
				# Facename = IR.Split_Img(img)
				Facename = IR.recognizeImg(img)
				print(Facename)
				resp_data = {'Recognized Person': Facename }

				#filename = secure_filename('gh')
				#img.save(os.path.join(app.config["IMAGE_UPLOADS"], filename))
				
				return json.dumps(resp_data)
```

## Face recognition function

```bash
def recognizeImg(IMG_PATH):

	#loading stored database pictures encoding
	encodeListKnown = pickle.load(open('save.pkl', 'rb'))
	#load img from path
	img = cv2.imread(IMG_PATH)
	#resize image for half of the image size
	imgS = cv2.resize(img,(0,0),None,0.5,0.5)
	#convert img to rgb image
	imgS = cv2.cvtColor(imgS, cv2.COLOR_BGR2RGB)

	#find face location from client uploaded image
	facesCurFrame = face_recognition.face_locations(imgS)
	#encode uploaded image
	encodesCurFrame = face_recognition.face_encodings(imgS,facesCurFrame)

	#find all the faces from loaded image
	for encodeFace,faceLoc in zip(encodesCurFrame,facesCurFrame):
		#compare database image encode with uploaded image encode
	      matches = face_recognition.compare_faces(encodeListKnown,encodeFace)
	      faceDis = face_recognition.face_distance(encodeListKnown,encodeFace)
	      #print(faceDis)
	      #match uploaded image with database image
	      matchIndex = np.argmin(faceDis)

	      #matching condition
	      if faceDis[matchIndex]< 0.50:
	          name = DE.classNames[matchIndex].upper()
          #unmatched condition
	      else: name = 'Unknown'

	      print(" ")

	      print('Recognized face:'+ " "+name)

	      print(" ")

	      #bounded box 
	      y1,x2,y2,x1 = faceLoc
	      y1, x2, y2, x1 = y1*2,x2*2,y2*2,x1*2
	      cv2.rectangle(img,(x1,y1),(x2,y2),(0,255,0),2)
	      cv2.rectangle(img,(x1,y2+20),(x2,y2),(0,255,0),cv2.FILLED)
	      cv2.putText(img,name,(x1,y2+15), cv2.FONT_HERSHEY_DUPLEX, 0.75, (255, 255, 255), 2)

	return name 
```

## License
A product of Shazzadul Islam
