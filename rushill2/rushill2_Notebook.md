# ECE 445 Project Notebook - Pet Health Monitor 
netid - rushill2

## Day 1 - 1/23/22
**Goal**: to finalize the topic and possible design
Decided to go with the pet harness and attempted to design it physically. We should be able to mount it on a harness, however the container must be small. We would have to see what weight cats can carry ethically in order to make sure we aren’t causing it discomfort

Could have possibly around 3-4 sensors. Temperature, heart rate and movement seem important, so we shall go with 3. Initial research showed us a really convenient temperature sensor the TMP36 (link). We found a 3-axis adafruit accelerometer, but would we need GPS too? Pulse monitor is the largest variable based on if it works or not. Would have to research into pulse-oximeter type sensor and see whether it can be clipped on the ear of the cat safely. 

Visualizing the entire design to look something like this hopefully:

[60f97af1fc253600181fca2f](https://user-images.githubusercontent.com/67648243/166885097-ee936946-f360-46b7-88a1-52c524875a9d.png)

## Day 2 - 2/2/22
**Goal**: Research to see actual plausability
Spoke to the partners and did some research. Found that accelerometer was very uncertain in telling you actual distance, but we could find some kind of qualitative metric based on change instead of static values. This seems worth exploring. Temperature is reliable. I think that especially because cats have fur it will be better insulated. Pulse sensor is the most problematic - will we be able to account for the damping via fur? On top of that, I think oximeters use IR and the fur would disable that. We would have to use something like an EKG. 


## Day 3 - 2/8/22
**Goal**: To modularize the design
Planned to determine important components and start thinking of a modular design for each. Narrowed it down to control unit, sensing unit, physical unit. If we get important data, we need to display it correctly somehow. LCD on device would make it heavier and more dangerous. Phone/laptop app would have to do.
Kind of a block diagram here, would have to review. Need to store the data before sending – would need SD-card if on chip memory is not enough.

![blockkkk](https://user-images.githubusercontent.com/67648243/167033805-2dddfcac-a4c7-4968-a667-50be1faeb8a7.png)

Connection was decided the be via Bluetooth and data would be parsed on an android app. Hope to get data in some form of string or array, but it would probably require some parsing on the microcontroller side before we can do that. 

Sensors would have to be placed optimally for the best readings. After inspection on a cat and some research (http://www.vetstreet.com/dr-marty-becker/check-your-cats-vital-signs-at-home), found that temperature and pulse behind the front legs would be best. Acceleration would have to be placed in such a way that it does not experience too many movements as the cat does its normal things. 

## Day 4 - 2/18/22
**Goal**: look for parts and tradeoffs

Sensing is quite clearly the most important. So we decided our sensors today, along with backups. The main sensors were as follows:
1.	Temperature – TMP36 (link)
2.	Accelerometer – ADXL343
3.	Pulse – Adafruit 1093
Temperature sensor has a good operating range for our purpose. Pulse sensor seems light and durable but would have to go through the datasheet well enough to check all requirements. Accelerometer gives acceleration data in 3 axes – could be used for finding time spent active and inactive if distance does not work out. 
As seen below, more or less the final positions of the sensors. 

Temp sensor would also work the best I feel since it had a thermocouple and that would be insulated by fur if placed right as you can see here. 

   ![612365723d652740479](https://user-images.githubusercontent.com/67648243/166885431-6c43401e-e55a-4f10-8c2e-8bec203d0527.png)

Pulse there was another one but that needed to be mounted onto a PCB it was a small pulse oximeter thing. Dont know if its a good idea to have another small PCB. 
        ![jdjd](https://user-images.githubusercontent.com/67648243/166885586-e0221d56-1295-484a-bb74-f68aca3d4ffd.png)



## Day 5 - 2/20/22
**Goal** : choose other components

Still need to figure out power supply and the actual microcontroller we use. Don’t want to have a Bluetooth module separately – would take more space – not ideal.
Options so far are – variants of ESP32(https://www.mouser.com/c/?q=ESP32), Bluefruit Feather 32u4(https://www.mouser.com/new/adafruit/adafruit-feather-32u4-bluefruit-le/), ATMega? (https://www.mouser.com/c/?q=atmega328)
Likely to go with ESP-32 due to nicer pinout and apparently less issues with programming. This would work with some kind of USB programmer I assume. Still open to discussion within the team. 
Power supply probably would turn out to be 3.3 or 3.7V since that falls in the operating range of all the sensors. SD-card reader module would also have to be picked such that it fits this power requirement. 

## Day 6 - 2/23/22
**Goal** : Make a list of specific features we want to implement.

For the app:
1.	Parse incoming data successfully
2.	Display metrics on app
3.	Be able to receive data from the SD-card (upper limit?)
4.	Not crash!

For the sensors:
1.	From temp sensor we want actual temp values. So we would need some ADC logic in the control unit.
2.	Accelerometer will be uncertain but we should try to minimize uncertainty. would 1/2 at^2 work
3.	Pulse we would want EKG like readings but again would have to do that in control unit

Control unit:
1. Handle all the SD card stuff, sensors parsing (ADC or SPI I/O setup)
2. power states 
3. Bluetooth transmission

Overall:
1. Need it to be very comfortable. So snug, but not too tight 
2. Can't have PCB overheat so need to take care with power calc and use oscilloscope
3. The transfer should be quick

## Day 7 - 2/27/22

**Goal**: Figuring out SPI communications and ADC logic

For ADC, it seems pretty straightforward and can follow the tutorial here : https://learn.adafruit.com/tmp36-temperature-sensor/using-a-temp-sensor

For SPI communication, I understand that there is a master-slave relation and one Din and Dout port each. LINK said something about there being the first 4 clock cycles for setup, but would have to look into that further. essentially need to initialize the ESP-32 (finally decided) to master and each sensor and SD-card as slave. ESP-32 has 3 SPI interfaces so that is great for us. 

![spi](https://user-images.githubusercontent.com/67648243/167034346-a8d65ebc-4d43-4ad8-a696-5597cfd6038f.jpg)

Seem to have thought of an algorithm for SPI initialization.

## Day 8 - 2/29/22
**Goal** - PCB Design
We started out PCB design today, and had all the upper layer schematic components set up. We did not yet actually create the board with the wirings on it, but made sure to get the upper layer set up. We created a dev board suited to our purposes. We had connectors for UART, power, IO pins and SPI pins for the microcontroller, along with buttons. Below is the schematic for what we designed. We did not do power yet

![hh](https://user-images.githubusercontent.com/67648243/167034995-6920392a-92ce-4d55-a5c9-601c99fd3173.png)


## Day 9 - 3/1/22

**Goal**: Figure out bluetooth 

Bluetooth on the microcontroller end is easy. We can use the SerialBT interface. Just need to being the BT serial port using begin() and then check for whether .read() returns a value. The problem on this end, I believe, will be setting up the android side. I read the documentation at https://developer.android.com/guide/topics/connectivity/bluetooth but it seems like a lot of the functions are deprecated. Not that I have android experience, but I'm going to figure it out today.

Each page is a fragment that is its own class (a subclass of the MainActivity). Now a class for bluetooth connection with all the functions for parsing the data in there too would work. Could instantiate a barebones dataless class in the fragment that I want to actually parse data in, decide where to go next, and then instantiate now a parametric bluetooth class that I would use to call the parsers. Seems like a decent idea.

## Day 10 - 3/5/22 

**Goal**: Actually look at wiring PCB and start possibly ordering parts

None of us have done kiCAD PCB design before, so would have to make sure that we do the PCB design right. We have to use footprints from the parts we need, so we would have to chart out the power unit schematic for the PCB too, to get regulators we need. Below is the schematic for the power unit we designed. 

![power](https://user-images.githubusercontent.com/67648243/167035165-075d5137-86c9-4a3c-ad00-d61efbbeeab0.png)

I assume RX and TX are for the connection to the programmer. If that's the case, then we'd need a lot of connectors on the board. For that, IO pins, and even power in. Cheap and available on Amazon, should not be an issue to get them whenever. ESP-32 should be ordered soon since they're in low supply it seems. I checked mouser and they had no stock so I went to another website that had stock. 

Would have to do actual PCB wiring later after verifying the power system working. 


## Day 11 - 3/8/22 

**Goal**: Pre-Break meeting

We planned on wiring the PCB once we met after the break ended. I would look into getting sources for our parts so we could order from the University account. I went to the website and figured out how to do the order. Made a list of all the parts. We knew we would need a regulator to 3.3V and I found the LM1117(https://www.ti.com/lit/gpn/lm1117) to be a good fit. Will use this along with buttons, plate resistors of 10k ohms, a 37uF capacitor and a Diode. Spoke to the owner of the cat we were planning on testing on. My roommate has a cat but he's too big and too furry, wouldn't be good for us. Plus the cat's a drama queen. 

## Day 12 - 3/20/22

**Goal**: App work

I decided to go ahead and work on the app. I set up a bluetooth Adapter that would allow me to get a bluetooth manager. Using this manager I would create a socket to the ESP-32 that would check for the MAC address of that device only. I would then try to connect to the socket. Once this happened, data that was staged on the SD-card would be sent in. I also figured out that arduino and android both are capable of directly streaming data, and that may be qay quicker than collection, storage and then sending. https://techtutorialsx.com/2018/03/13/esp32-arduino-bluetooth-over-serial-receiving-data/ showed me a little more on how to do that. 

So far I had this code done: 

        BluetoothAdapter Mbt = BM.getAdapter();
        String deviceName = "Cat Thermometer";
        BluetoothDevice result = null;
        Set<BluetoothDevice> devices = Mbt.getBondedDevices();
        if (devices != null) {
            for (BluetoothDevice device : devices) {
                if (deviceName.equals(device.getName())) {
                    result = device;
                    Log.i("deviceName", result.getName());
                    break;
                }
            }
        }
        if (result != null){
            Log.d("BT", "Trying Socket...");
            UUID MY_UUID = result.getUuids()[0].getUuid();
            socket=result.createRfcommSocketToServiceRecord(MY_UUID);
            this.serverSocket = Mbt.listenUsingRfcommWithServiceRecord("test", result.getUuids()[0].getUuid()); // 1
            Log.d("BT", "Got Socket!");
        } else{
            this.socket = null;
            this.cancelled = true;
            Log.d("bluetooth", "failed bt socket");
        }
This would create a socket object I could try to connect to. Also added some basic buttons on the home screen for pairing. Would still need to know the MAC address of the thing. 


## Day 13 - 3/21/22 
**Goal**- More app work

Fleshed out a good looking front-end. changed the background, colors and cleaned up the code to improve socket creation latency and load times. Did some further work on the bluetooth side and set up this small chunk of code in the BluetoothHandler Class

        this.cancelled = false;
        if (Mbt.isEnabled()){
        } else{
            Mbt.enable();
        }
        byte[] buf = new byte[2048];
        int bytes;
        OutputStream tmpOut = null;
        InputStream tmpIn = null;
        if(socket!=null){
            Log.d("BT", "Connecting...");
            socket.connect();
            Log.d("BT", "Connected!!"); }
            
So ideally this could should show me the connection status and connect to the device. I believe there hasn't been much work done on the arduino end therefore would have to check with the teammates and try to get MAC address. Additionally, I believe that the next step here would be to get something called an inputStream I saw in the documentation here - https://developer.android.com/reference/java/io/InputStream

Below is an image of home screen front-end
![appdesign](https://user-images.githubusercontent.com/67648243/167036506-6560e0da-9b0b-4708-9cdd-946171614ca0.png)

## Day 14 - 3/24/22

**Goal**- EVEN MORE app work

Created the second page and page layouts for the Movement, Pulse and Temperature Data. Learned som basic XML in order to create TextViews, but will have to learn how to assign them programmatically and create graphs (maybe? might not do if too hard)

Below is the design for these pages. Made one for the loading screen while we wait for the data to come in from SD-card

![appd2](https://user-images.githubusercontent.com/67648243/167038281-10d85749-540b-49d7-9172-79abd1eeb273.png)
![appd3](https://user-images.githubusercontent.com/67648243/167038285-0b4d091f-79c8-4ec8-98dc-04b91e416bf3.png)
![appd4](https://user-images.githubusercontent.com/67648243/167038288-2f62d46d-d457-41bd-aac7-46bf1b73a585.png)


## Day 15 - 4/27/22

**Goal**- FINALLY connecting to bluetooth

Ran a simple script that would create a fake bluetooth device and then tried to connect to that MAC as the endpoint. I set up the functions for getting the input from the device, and on talking to Tanmay and Jeff we figured that we would get a stream of raw bytes that I would have to convert to a string or tuples in order to make the data parseable. 

Below is code for the inputStream setup. Curious to check outputStream and see if we can send Arduino a kill-switch from the phone. 

        int i = 0;
        int inval = 0;
        while(true){
            boolean flag = false;
            String message = null;
                try{
                    if(i==10){
                        break;
                    }
                    tmpIn = this.socket.getInputStream();
                    inval = tmpIn.read(buf);
                    message = new String(buf, 0, inval);
                } catch (IOException e) {
                    e.printStackTrace();
                }
                if(message.contains("ece445") | inval == 4){
                    return true;
                }
                else{
                    proc(message, inval, m, i);
                }
                i += 1;
        }
        return true;
    }

Think of proc as a black box function for now that sends parses the data and sends it to handler functions for each one. 

## Day 16 - 4/4/22
**Goal**- status updates and figuring out where we are

Turns out we are rather behind with the PCB development. Ordered parts but did not do the same for resistors and capacitors. Other commitments prevented e from working on 445 for a while. Worked with Jeff further on the app, once the bluetooth connection was setup, debugging the code was ridiculously hard without the actual device to pair with. Decided to try to beautify the app and try to research on some graph XML layouts https://www.infoworld.com/article/3226733/graphlib-an-open-source-android-library-for-graphs.html


## Day 17 - 4/8/22

**Goal**- Debug the app
More app work and debugging was a tedious process. We found that we were using a deprecated library and had to switch to CreateRFComm but with an insecure frequency. Security was not our concern for this design so we made changes to the code and saw progress. However we were still unable to test. Parts seem to be well on their way so we planned soldering duties and programming the microcontroller. SPI communications code was needed along with a generic state machine to handle SD-Card reads and writes. https://www.arduino.cc/reference/en/libraries/statemachine/

## Day 18 - 4/18/22

** Goal **- Start PCB development
With the parts coming through late we finally got to solder. Soldered all the components on until we realized that the regulator we ordered was a REG1117 which was larger than an LM1117. Thankfully we found a TA with the regulator. Some of our resistors did not come through either, so instead of plate resistors, we would try to use breadboard resistors since we were crunched on time. We soldered all connectors, diodes, capacitors, resistors, and microcontroller on. Only need the UART soldered and did not hae female-female cables for that. Ended up with the PCB as seen here

![peeseabee](https://user-images.githubusercontent.com/67648243/167041203-3bd328ef-fbb8-4cb5-bdef-df9ddb033bf9.png)

# Day 19 - 4/21/22
**Goal**- App work

I focussed on the app while jeff and tanmay started coding the microcontroller. Finally could test and the bluetooth worked! needed to test for input streams coming in. Once the data was received, I found that it was a string with tabs between each type of reading and spaces between axis values for the ADXL343. I did a simple split and parsed them into arrays in code seen below and then wrote the handler for temperature today. So far only max and average values are found. 

Code for the parser:
```
          public void proc(String data, int numbytes, MainActivity m, int i){
              String[] usestring = null;
      //        Log.d("NumBytes", String.valueOf(numbytes));
              String[] accvals = new String[numbytes];
              float[] tempx = new float[numbytes];
              float[] tempy = new float[numbytes];
              float[] tempz = new float[numbytes];
              float[] tempv = new float[numbytes];
              // parse data string into
              usestring = data.split("\t", 3);
              tempv[i] = Float.parseFloat(usestring[1]);
              accvals[i] = usestring[0];
              // parse accelerometer axis data
              String[] split = accvals[i].split(",", 3);
              tempx[i] = Float.parseFloat(split[0]);
              tempy[i] = Float.parseFloat(split[1]);
              tempz[i] = Float.parseFloat(split[2]);
              m.setTempvals(numbytes, tempv, i);
              int zerocnt = 0;
              m.setYlist(numbytes, tempy, i);
              for (float f: m.ylist){
                  if(f==0){
                      zerocnt += 1; }}
              float[] dd = new float[30];
              int j = 0;
              int k = 0;
              for (float f:m.ylist){
                  if(f!=0){
                      dd[j] = f;
                      j += 1; }}
              m.setYlist(numbytes, dd, i);
              int n = 0;
              float[] dk = new float[30];
              m.setZlist(numbytes, tempz, i);
              for (float f:m.ylist){
                  if(f!=0){
                      dk[n] = f;
                      n += 1; }}
              m.setZlist(numbytes, dk, i);
              float[] dy = new float[30];
              int p = 0;
              m.setXlist(numbytes, tempx, i);
              for (float f:m.ylist){
                  if(f!=0){
                      dy[p] = f;
                      p += 1; } }
              m.setXlist(numbytes, dy, i);
          }

```

Code for the Temp Handler (jeff helped)
```
   public float[] tempHandler(SparkView sparkView, MainActivity m){
        float[] usearr = m.gettempvals();
        float sum = 0;
        float max_ = 0;
        float min_ = 1000;
        float[] actualarr = new float[usearr.length];
        int i = 0;
        int zerocnt = 0;
        for (float f:usearr){
            if(f!=0){
                actualarr[i] = f;
                i += 1; }
            else{
                zerocnt += 1; }
            sum += f;}
        for (float f:usearr){
            if(f>max_){
                max_ = f;}
            if(min_>f){
                min_ = f;}}
        float avg = 0;
        if(usearr.length!=0){
            avg = sum/(usearr.length-zerocnt);
        }
        if(sparkView!=null){
            sparkView.setAdapter(new MyAdapter(usearr));
        }
        float[] ret = new float[2];
        ret = new float[]{avg, max_};
        return ret;
    }
    
  ```
  
  ## Day 20 - 4/20/22
  **Goal**- Fix the PCB issue hopefully
  
  Our PCB burnt out. We could program it, but the code never ran on it. While it was connected to the battery, we smelled some burning. I would say that is due to the dangling resistors that it burnt. its possible that a capacitor overloaded due to the resistors briding on our design. As you can see below the plate resistors would have worked, but we did not have time to get them. Below is the PCB design - we also realized that we missed a connection from the ESP to the SPI Clock port for one of the sensors. Instead of ordering a new PCB (too little time), we tried to use the dev-board for ESP-32 that we were testing on. 
  
Dev board is easy to use, and all we needed to do now was test on the cat for different sensor data.
  
  ![pcbdesign](https://user-images.githubusercontent.com/67648243/167041886-a70f65b1-589d-4721-aea3-0ab528d02bce.png)


 ## Day 21 - 4/21/22
  **Goal**- Design changes
  
  Discussed and figured out that the SD-Card was unnecessary. We could actually transmit bluetooth data real time useing the BLE or the SerialBT modules. We decided to go with SerialBT and with some quick setup everything worked fine. Below is the serialBT code setup with the mac address of the device saved
  
  ```
  #include "BluetoothSerial.h"

BluetoothSerial SerialBT;

String MACadd = "10:97:BD:D4:9A:9C";

void setup() {
  Serial.begin(115200);
  //SerialBT.setPin(pin);
  SerialBT.begin("Cat Thermometer"); 
  //SerialBT.setPin(pin);
  Serial.println("The device started in master mode, make sure remote BT device is on!");

void loop() {
  int coumter = 0;
  if (Serial.available()) {
    SerialBT.write(Serial.read());
  }
  if (SerialBT.available()) {
//    SerialBT.write((int)"Tanmay is gay");
  }
  String a = "page1oktt";
  uint8_t buf[a.length()];
  memcpy(buf,a.c_str(),a.length());
  SerialBT.write(buf,a.length());
  SerialBT.println(); 
  delay(1000);
  if (coumter > 45)
    SerialBT.disconnect();
}
```


 ## Day 21 - 4/23/22
  **Goal**- Testing
  
  We tried using the temperature sensor only (not the entire system) on a cat and found the values to be acceptable. For acceleration, the change in axis acceleration actually gave us an interesting metric to try out. The pulse sensor however, did not work at all. We assumed that it was faulty because it did not work on us and used the backup. The problem with the backup was that it required an sticky gel to be used. I tried that on my arm, and some hairs came off. Would not want to use that on the cat. So abandoned it altogether. 
  
  I finally set up a decent acceleration handler that would take in the 3-axis values and give us an Activity Ratio, based on the delta state being greater than some threshold we decided after testing. 
```
  public float[] accHandler(MainActivity m){
        float[] tempx = m.getXvals();
        float[] tempy = m.getYvals();
        float[] tempz = m.getZvals();
        float value = 0;
        float active = 0;
        float inactive = 0;
        float oldx = 0;
        float oldy = 0;
        float oldz = 0;
        float useval = 0;
        for (int i = 0; i<tempx.length; i++){
            if(Math.abs(tempx[i]-oldx)+ Math.abs(tempy[i]-oldy)+Math.abs(tempz[i]-oldz)>2){
                active += 1;
            }
            else{
                inactive += 1;
            }
            float ret = (float) Math.sqrt(Math.pow(Math.abs(tempx[i]-oldx), 2) + Math.pow(Math.abs(tempy[i]-oldy), 2) + Math.pow(Math.abs(tempz[i]-oldz), 2));
            ret = ret/9;
            useval += ret;
            value = (tempx[i] + tempy[i]);
            oldx = tempx[i];
            oldy = tempy[i];
            oldz = tempz[i];
        }
        float activity_ratio = active/(inactive);
        if(activity_ratio>0.25) activity_ratio += 0.2;
        if(activity_ratio<0.1) useval = 0;
        float[] sus = new float[]{activity_ratio, useval};
        return sus;
    }
```

## Day 21 - 4/25/22

**Goal**- Final testing of the device pre-demo

We are done! built everything, so decided to test on the cat. Cat seems comfortable and seems to not be bothered by the device. It is a little bulky, but that's because we couldn't find a container light enough for it. Temperature readings were great and accelerometer data did actually give us an indication of how active the cat has been. We had to use a weird box since our initial design for that did not work out. However, here is a picture of the cat wearing it. 

![catto](https://user-images.githubusercontent.com/67648243/167042789-17aff4d9-4cba-4040-bd79-35788bd1ee58.png)

## Acknowledgements
1. Hojoon Ryu (TA) for constant guidance and support throughout
2. Pengfei Song (Professor) for pre-empting the pulse sensor issue and directing us to use accelerometer data for qualitative results
3. Sanjana Sastry (Test Subject Owner) for letting us test on Jasper!
