<h1> Create custom interface for Raspberry pi 4 with qt5 creator</h1>

Based on the comprehensive youtube video titled : "Qt for Raspberry Pi - Qt 5.14.2 cross compilation for Raspberry Pi 4 model B - Run Qt on Raspberry" by Ulas Dikme : https://youtu.be/TmtN3Rmx9Rk


<h2>Prerequisites</h2>
<ul>
  <li>Raspberry pi 4 model B (Rpi 4B)</li>
  <li>Minimum 16 Gb micro SD card</li>
  <li>Screen connected to Raspberry pi</li>
  <li>Recommended Raspberry pi 4 model B power adapter</li>
  <li>Raspberry pi IP number reserved via your router dashboard</li>
  <li>Laptop connected the same network as your rpi 4B (PC, MAC or Linux)</li>
</ul>

<h2>Installation overview</h2>

<ul>
  <li>Prepare Raspberry Pi for compilation</li>
  <li>Setup Ubuntu 20.04 virtual machine and prepare for compilation</li>
  <li>Install QtCreator and deploy compiled example</li>
</ul>


<h3>Write Raspberry Pi OS to SD card</h3>
<ul>
  <li>Download Raspberry Pi Imager : https://www.raspberrypi.org/downloads/, select imager based on your operating system</li>
  <li>Install software</li>
  <li>Run Raspberry Pi Imager, choose "Raspberry Pi OS" with Raspberry pi Desktop and write to SD card</li>
  <li>Insert SD card into the rpi and boot Rpi</li>
</ul>

<h3>Basic Rpi setup</h3>

<p>Once your Rpi has booted, you will be greated with the installation welcome screen.  Go through the following steps :
<ul>
  <li>Click on next</li>
  <li>Set Country, language, timezone.  Click Next.</li>
  <li>Set your password</li>
  <li>Connect to your wifi network.  HOWEVER IT<S RECOMMENDED TO CONNECT VIA A WIRED CONNECTION.</li>
  <li>Skip the following steps without updating, we will do this in the following steps.</li>
</ul>

<h3>Open a terminal window</h3>
<p> Insert the following commands :</p>
<pre><code>sudo apt update
sudo apt-get upgrade
</code></pre> 

