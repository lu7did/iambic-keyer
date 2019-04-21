# 	iambic-keyer

        This keyer was adapted using Phil's verilog code from
        the openHPSDR Hermes iambic.v implementation to build
        and run on a raspberry PI 3 or many other SBC's.

        Check out the GPIO assignments in iambic.c and suite them
        to your board.

        LU7DID wiring
 
             GPIO 12 Key Out
             GPIO 19 Speed sensor
             GPIO 13 Dash
             GPIO 15 Dot
   

        This code implements an Iambic CW keyer.  The following features are supported:

                * Variable speed control from 1 to 60 WPM
                * Dot and Dash memory
                * Straight, Bug, Iambic Mode A or B Modes
                * Variable character weighting
                * Automatic Letter spacing
                * Paddle swap

        Dot and Dash memory works by registering an alternative paddle closure whilst a paddle is pressed.
        The alternate paddle closure can occur at any time during a paddle closure and is not limited to being
        half way through the current dot or dash. This feature could be added if required.

        In Straight mode, closing the DASH paddle will result in the output following the input state.  This enables a
        straight morse key or external Iambic keyer to be connected.

        In Bug mode closing the dot paddle will send repeated dots.

        The difference between Iambic Mode A and B lies in what the keyer does when both paddles are released. In Mode A the
        keyer completes the element being sent when both paddles are released. In Mode B the keyer sends an additional
        element opposite to the one being sent when the paddles are released.

        This only effects letters and characters like C, period or AR.

        Automatic Letter Space works as follows: When enabled, if you pause for more than one dot time between a dot or dash
        the keyer will interpret this as a letter-space and will not send the next dot or dash until the letter-space time has been met.
        The normal letter-space is 3 dot periods. The keyer has a paddle event memory so that you can enter dots or dashes during the
        inter-letter space and the keyer will send them as they were entered.

        Speed calculation -  Using standard PARIS timing, dot_period(mS) = 1200/WPM

        NOTES:
        In iambic.c set SIDETONE_GPIO to 0 to use the PI's built-on audio out jack
        for sidetone OR an actual wiringPi GPIO number for the softToneWrite() sidetone.  

        This version of iambic-keyer uses Jack Audio Connection.  
        This is a good intro for getting Jack setup on the PI:  
           http://wiki.linuxaudio.org/wiki/raspberrypi#audio_software_repository
        execute with
           jackd -P70 -p16 -t2000 -dalsa -dhw:1,0 -p512 -n4 -r48000 -s
        You need to run this using sudo to use the pigpio functions.  

        ====================================================================================================
        keyer {start | stop | reload}
  
        Script to enable and disable the keyer at startup 

        To launch automatically at reboot add the following line to crontab

        @reboot sudo /home/pi/iambic-keyer/keyer start > /home/pi/cronjoblog 2>&1
