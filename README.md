# hello-world
my test repository

This is my flowerpot man program in python
I'm a total noob with github
#!/usr/bin/env python
#Dave Scales
#Weather forecast reader - uses feedparser, raspberry talk and espeak


#remember to set amixer cset3 1 to set audio to 3.5 jack

import RPi.GPIO as GPIO
import feedparser
import os
import espeak
import sys, subprocess, urllib, time

time.sleep(60)
 
def getSpeech(phrase):
    googleAPIurl = "http://translate.google.com/translate_tts?tl=en_gb&"
    param = {'q': phrase}
    data = urllib.urlencode(param)
    googleAPIurl += data # Combine the data from the Google API
    return googleAPIurl
 
def raspberryTalk(text): # This will call mplayer and will play the sound
    subprocess.call(["mplayer",getSpeech(text)], shell=False, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    

def say(something):
    subprocess.call(['espeak', '-ven-n+f2', '-k20', '-p20', '-s150', something])


# Set up the GPIO pins

GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)

GPIO.setup(18, GPIO.OUT)
GPIO.setup(11, GPIO.IN)

while True:

    # Loop until an input is detected on pin 11 and turn on LED on pin 18
    while 1:
        if GPIO.input(11):
             GPIO.output(18, True)
             break
        else:
            GPIO.output(18, False)

    
    say("Hey Up, Hope you are having a good day. I am the talking flowerpot and I am from Yorkshire")
    time.sleep(0.2)

    say("I am gunna give thee the weather forecast for Settel")
    time.sleep(0.2)


    #pulls all the data down
    rss_link ='http://open.live.bbc.co.uk/weather/feeds/en/2638192/3dayforecast.rss' #add your area code
    d = feedparser.parse(rss_link)


    #Parse data in various elements
  
    
    forecast_today = d.entries[0].title
    forecast_tom = d.entries[1].title

    #change string to ascii
  
 
    forecastAscii = forecast_today.encode('ascii','replace')
    forecastAsciiTom = forecast_tom.encode('ascii','replace')
    

    say("Here is the weather for today ")
    time.sleep(0.5)
    raspberryTalk(forecastAscii)
    time.sleep(0.5)


    say("Here is the weather for tomorrow ")
    time.sleep(0.5)
    raspberryTalk(forecastAsciiTom)
    time.sleep(0.5)
    

    say("I am able to talk because I have a Raspberry Pi brain")
    time.sleep(0.3)

    say("Thanks for listening. See thee later")


  

   
