## 1/22
**Goal**: Brainstorm Ideas\
Today we brainstomed several idea such as a drone to take measurements for a forest fire or general weather. I was partial to the idea of an alarm clock that would heat your clothes up in the morning, so you can wear warm clothes as soon as you wake up, since this is something i struggled with back home where my house is about 65 degrees when i wake up.

## 2/2
**Goal**: Finalize Ideas\
Since the previous alarm clock idea was denied, for dubios reasons in my opinion, we decided to go with a cat thermometer. We formalized the project and submitted for project approval. The invention will look like a small box attached to a harness people use to walk their dogs/cats. The sensor will either touch the cat or be mounted on the box.
![image](https://user-images.githubusercontent.com/71428266/167022797-7bb4e728-d0fc-4bec-91bd-af234d0d8a79.png)

## 2/10
**Goal**: Do proposal and research idea\
We wrote up the proposal and did research to what sensors we could use.

We decided we could have possibly around 3-4 sensors. Temperature, pulse rate, acceleromter/gps seemed lik the best. Initial research showed us a really convenient temperature sensor the TMP36. 
![image](https://user-images.githubusercontent.com/71428266/167022615-3a6e1410-0f59-4e08-bb03-d34e8b9d02a7.png)
This will work well, and I also found a web resource regerding converting the skin temp to a core body temperature. It points out areas on the cat that will work best.
![image](https://user-images.githubusercontent.com/71428266/167023715-32bc6714-7bca-48d6-b9df-5a9aa3d0c702.png)



We also went ahead and made a block driagram to define the different parts of the project and how they integrate. This is seen below.
![block](https://user-images.githubusercontent.com/71428266/167023316-daf0366f-c28a-4f3a-a6bf-28a19d7780ae.png)

As you can see we decided on blutooth. We decided pretty quickly because it seems like the best option. WE have the three sensors we chose, and a power subsystem.

## 2/20
**Goal**: look for parts and make shopping list

Sensing is quite clearly the most important. So we decided our sensors today, along with backups. The main sensors were as follows:
1.	Temperature – TMP36 (link)
2.	Accelerometer – ADXL343
3.	Pulse – Adafruit 1093
Temperature sensor has a good operating range for our purpose. Accelerometer gives acceleration data in 3 axes – could be used for finding time spent active and inactive if nothing else, as our professor mentioned. Professor also mentioned he was very concerned about pulse sensor because they are designed for humans not cats. This is a valid concern, but we found someone who was able to get pulse data from sheep, which seem to have more fluff than a typical cat.

## 2/22

**Goal** : choose other components

We wanted to choose the microcontroller and on the advice of a friend we went with esp32 which has onboard memory and a built in bluetooth chip. This runs on 3.3 V so we also picked out a cheap battery for it to run. We know we need a regulator still but will get that when we design the pcb. We also picked out the SD card, which will be more than large enough for what we are using it for. In additon we needed an SD to pin converter so we can interface as well as some buttons.

## 2/24
**Goal** : App work
I talked with rushil because he has an android phone and seemed to know more about it then I. He said it wouldnt be very hard to build a basic app with bluetooth functinality. We have a rough idea of what the app will look like now, as well as the knowledge we need to use android sdk to make it. It will basically be a page with a pair button, which will pair, and then a download data button which will show the user the data from the last session(before the data was downloaded last to a maximum of 24 hours).

## 2/29
**Goal** : PCB board work
We decided it would be a good idea to finish the inital PCB design for the first review since none of use had ever made one before and we could use the feedback. We found a useful guide on the ESP32 specifically, but it was using some outdated version of our pcb software, so this took some figuring out. We have a finsihe pcb design now, so this is good.

## 3/8
**Goal** : Implement changes to PCB
Our TA Hojoon told us that it looked ok but it had to be smaller, i think 10x10. This wasn't a huge deal, mainly just a pain in the ass since we had to redraw most the wire conenctions since they were broken in rearranging the board to make it smaller. Tanmay actually did this and submitted for first round pcb ordering. Rushil said he'd do the ordering before spring break. I don't think any of us are planning to work during spring break, but we should be in a decent spot.

## 3/23
**Goal** : order parts
There was a communicaiton mishap and nobody ordered the parts. I went ahead and did that, and Hojoon quicly approved them, which is good. We are a little behind now but not terribly. Ordering was mostly just tedious because of having to use the univeristy system in order to use their money. We ordered everything and should be getting the PCB soon.

## 4/4
**Goal** : do work
We didn't do work for quite a while, and when I talked to tanmay realized we didnt have the pcb board components. I stupidly assumed that when we sent the pcb files to be manufactured they would also include the small parts(resitors, capacitors etc.) that we listed in the files. Nope, so we had to order these online before any actual work can begin, besides the app of course.

## 4/8
**Goal** : do app
 Rushil and I started work on the app. Making the framwork wasn't to bad but making it work with blutooth was very much bad. The documentation and libarires to do so was pretty outdated without any tuturials up to date enough to be of any use. We still made decent progess but the app is a sticking point.
 
## ~4/18
**Goal** : work on pcb
WE started the actual soldering work on the pcb. Sodlering the esp32 was a huge pain because of the odd way it touched the board. Instead of prongs it bascially sat there, and I had to solder in the hole in betweem to make it work. We found out that the capitor and regulators we ordered were the wrong size so we had to scrounge around the lab looking for viable replacements. Everything ended up soldering though and once the programmer, we didn't know we needed. comes in the mail we can test it.
## ~`4/22
**Goal** : General work
We recieved the programmer and it does program. I begain the code to do SD card reading, controll flow, and tthe sensor code in arduino. Rushil mainly is working on the app to get that operational. The code hasn't been tested, but most of the content is there as well as it compiling.

## ~4/24
**Goal** : General work
It's a bit rough, but PCB won't work. Either it never worked, or we burn it out somehow. I got a minor burn when I grabbed the battery which was hooked up to the pcb, which is why I think it may have burnt out, because batteries that burn is generally not a good sign. We lucked out though and a team had an extra esp32 dev board they weren't using which we can use for development. We abandones the SD card at this point since the DEV board should have more than enough space to make it work and getting the sd card to work was proving difficult. App work was still continuing in the background of this.

## ~4/25
**Goal** : General work
WE got all sensors working pretty early on so we tested with the cat. We were able to tell pretty early on that some of the sensros we ordered were just garbage so didn't even bother with those. The pulse sensors didn't even work on humans so we obtained an EKG sensor. This worked on humans, but relied on a sticky adhesive to work and we were uncomfortable putting this on a cate so we deiced to give up on pulse sensor We then needed to integrate witht the App so a large portion of time was spent on the app. While this was going on, we started physcialy constructing the device so it would be ready when the app was. We worked very late into the night and had to scrap a box together for the dev board because it was a different size thean the planned pcb so we didnt have one that could work.
