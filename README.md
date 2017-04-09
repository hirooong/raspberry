first commit


https://youtu.be/Z0_GJtAo2_M

#########mission2
#!/usr/bin/env python 

# Log a temperature probe DS18B20 connected to Raspberry Pi  to a Thingspeak Channel 
# To use, get a Thingspeak.com account, set up a channel, and capture the Channel Key at https://thingspeak.com/docs/tutorials/  
# Dr Ian Pun lifted code from various sources - it works!   Sept 16, 2016 
# Just change the field4 to your field name and your Thingspeak API key. 
# go to your channel and your data is plotted there! 

import httplib, urllib 
import time 
import datetime 

import os 
import glob 
import time 

os.system('modprobe w1-gpio') 
os.system('modprobe w1-therm') 
base_dir = '/sys/bus/w1/devices/' 
device_folder = glob.glob(base_dir + '28*')[0] 
device_file = device_folder + '/w1_slave' 

def read_temp_raw(): 
        f = open(device_file, 'r') 
        lines = f.readlines() 
        f.close() 
        return lines 

def read_temp(): 
        lines = read_temp_raw() 
        while lines[0].strip()[-3:] != 'YES': 
                time.sleep(0.2) 
                lines = read_temp_raw() 
        equals_pos = lines[1].find('t=') 
        if equals_pos != -1: 
                temp_string = lines[1][equals_pos+2:] 
                temp_c = float(temp_string) / 1000.0 
                
                return temp_c 


key = 'VXTVWPGVWPUY55U2'  # Thingspeak channel to update 


def thermometer(): 
    while True: 
        #Calculate CPU temperature of Raspberry Pi in Degrees C 
        key = "VXTVWPGVWPUY55U2" # Thingspeak channel to update 

        
        now = datetime.datetime.now() 

        temperature4 = read_temp() 

        params = urllib.urlencode({'field1': temperature4, 'key': key}) 
        headers = {"Content-typZZe": "application/x-www-form-urlencoded","Accept": "text/plain"} 
        conn = httplib.HTTPConnection("api.thingspeak.com:80") 
        try: 
            conn.request("POST", "/update", params, headers) 
            response = conn.getresponse() 
            print temperature4 
            print response.status, response.reason 
            data = response.read() 
            conn.close() 
        except: 
            print "connection failed" 
        break 

if __name__ == "__main__": 
        thermometer() 
        
        
        
        
       
################### mission 4,5
mysql -uej -hlocalhost data -p
