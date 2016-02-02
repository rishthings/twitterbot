import time
import RPi.GPIO as GPIO
from twython import TwythonStreamer

# Search terms
TERMS = '#yes'

# GPIO pin number of LED
LED = 22

# Twitter application authentication
APP_KEY = 'WkA89UuTW8xo1S3T79YVTrdB1'
APP_SECRET = 'Gnqo0ZG1qOvNIh37ABaSmvRrs2GSnym6bX1zA7x54ANUO7ggpp'
OAUTH_TOKEN = '3746267535-YRYbz73KefrrCMx4th5mpNgehqTgojvpooo86H3'
OAUTH_TOKEN_SECRET = 'f6kvChri2444B8uhNG9bDO7hcqnJoZwdExfOvLOJ66ETy'

# Setup callbacks from Twython Streamer
class BlinkyStreamer(TwythonStreamer):
        def on_success(self, data):
                if 'text' in data:
                        print data['text'].encode('utf-8')
                        print
                        GPIO.output(LED, GPIO.HIGH)
                        time.sleep(0.5)
                        GPIO.output(LED, GPIO.LOW)

# Setup GPIO as output
GPIO.setmode(GPIO.BOARD)
GPIO.setup(LED, GPIO.OUT)
GPIO.output(LED, GPIO.LOW)

# Create streamer
try:
        stream = BlinkyStreamer(APP_KEY, APP_SECRET, OAUTH_TOKEN, OAUTH_TOKEN_SECRET)
        stream.statuses.filter(track=TERMS)
except KeyboardInterrupt:
        GPIO.cleanup()
