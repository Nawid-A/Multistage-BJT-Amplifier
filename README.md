# Multistage-BJT-Amplifier
A multistage BJT Amplifier designed from scratch using core physical principles of transistor amplifiers and behaviors to meet a specified set of specifications.

Preface:  

BJT amplifiers are one of the most common technologies in everyday appliances, like adjusting the volume on a speaker or regulating voltage to an extent. The basic premise of a BJT amplifier is to take in a small input signal and magnify it into a much larger signal. There are three types of BJT amplifier configurations, common emitter (CE), common base (CB), and common collector (CC, or better known as an emitter follower), each of which having their own distinctive characteristics and practical uses. The CE configuration is probably the most popular due to its high voltage and current gain capabilities, moderate input impedance, and high output impedance. The CB amplifier offers a high voltage gain, but very low current gain, as well as having a low input impedance and high output impedance. Lastly, the CC amplifier has almost no voltage gain, with its value being slightly under unity, but it does have a high current gain and very high input impedance, allowing it to be great for impedance matching and as a buffer for another amplifier. With this knowledge in mind, the goal of this project is to incorporate the properties of each of these amplifier configurations into a single cascading BJT amplifier that meets a given set of specifications.


Specifications: 
 
The following is the list of specifications that must be satisfied by our multistage amplifier design:  

• Power supply: +𝟏𝟎𝑽 relative to the ground;
• Quiescent current drawn from the power supply: no larger than 𝟏𝟎 𝒎𝑨;
• No-load voltage gain (at 1 𝑘𝐻𝑧): |𝑨𝒗𝒐| = 𝟓𝟎 (± 𝟏𝟎%);
• Maximum no-load output voltage swing (at 1 𝑘𝐻𝑧): no smaller than 8 V peak to peak;
• Loaded voltage gain (at 1 𝑘𝐻𝑧 and with 𝑅𝐿 = 1 𝑘Ω): no smaller than 𝟗𝟎% of the no-load
voltage gain;
• Maximum loaded output voltage swing (at 1 𝑘𝐻𝑧 and 𝑅𝐿 = 1 𝑘Ω): no smaller than 4 V peak 
to peak;
• Input resistance (at 1 𝑘𝐻𝑧): no smaller than 𝟐𝟎 𝒌𝜴;
• Amplifier type: inverting or non-inverting;
• Frequency response: 20 Hz to 50 kHz (−𝟑𝒅𝑩 response); 
• Type of transistors: BJT;
• Number of transistors (stages): no more than 3;
• Resistances permitted: values smaller than 𝟐𝟐𝟎 𝒌𝜴 from the E24 series;
• Capacitors permitted: 𝟎. 𝟏 𝝁𝑭, 𝟏. 𝟎 𝝁𝑭, 𝟐. 𝟐 𝝁𝑭, 𝟒. 𝟕 𝝁𝑭, 𝟏𝟎 𝝁𝑭, 𝟒𝟕 𝝁𝑭, 𝟏𝟎𝟎 𝝁𝑭, 𝟐𝟐𝟎 𝝁𝑭;


Proposed Solution: 

To meet the stated specifications, a two-stage amplifier design consisting of a CE amplifier and a special form of an emitter follower known as a class AB push-pull amplifier was selected. A schematic of the proposed design is shown below. 

![Screenshot 2024-10-04 001737](https://github.com/user-attachments/assets/886f6994-f958-47f7-87bf-9ea2ea174dec)

This design was chosen due to its minimal amount of components and stability. The first stage of the multistage amplifier, the CE amplifier, will be responsible for the entirety of the gain production (since the emitter follower has a gain close to unity), and for satisfying the 20kΩ minimum input resistance requirement by setting the values of the base resistors, RB1 and RB2, to very high values (which also minimizes the loading effect’s impact). In addition, a bypass capacitor was chosen to be implemented in order to raise the gain and output voltage swing to the specified values, but as a drawback, it makes the amplifier much more prone to distortions, so it was necessary to introduce another resistor, RE2, connected in series with the capacitor to prevent clipping from occurring.  

The next stage of the multistage amplifier, the push-pull amplifier (specialized emitter follower), serves the purpose of keeping the output voltage consistent between the no-load and loaded cases, which is thanks to its near unity voltage gain. However, if the base resistors of the emitter follower are not sufficiently high enough such that their parallel combination is lower than the output resistance from the CE amplifier, then the voltage supplied from the output of the CE amplifier to the base of the emitter follower will be reduced, resulting in a decrease of gain and swing. Hence, RB3 and RB4 need to be very high in order to prevent this from occurring. Further information and explanations will be provided in the next section discussing the design process of each stage in detail. 


Stage 1: CE Amplifier Design 
 
The following circuit shown in Figure 2 is the calculated CE amplifier obtained for stage 1 of the multistage amplifier. We will go through each step of the calculations to explain how this design was realized. It’s best to have the manual calculations pdf open while reading through the steps to fully understand where some of the equations and values used came from. Also, some preliminary assumptions made were that 𝛽 = 100 and that VBE= 0.7V.

![Screenshot 2024-10-04 002004](https://github.com/user-attachments/assets/c0b1e209-48ea-49fb-8c81-725a323dd9fe)

Steps: 

Calculation of RC and RE1 Values 
The first step to designing the CE amplifier is determining the appropriate values for RC and RE1. To do this, we will consider only the DC operation of the transistor, which means that all capacitor paths are open circuits, resulting in the base npn transistor shown below in Figure 3.  
 
![Screenshot 2024-10-04 002255](https://github.com/user-attachments/assets/561854c7-07d3-4ff0-a2d8-1952605572aa)
Figure 3: DC Equivalent Circuit of CE Amplifier
 
Now, lets select some arbitrary value for RC, like 27kΩ. Since the bypass capacitor is not present in the DC equivalent circuit, the gain of the CE amplifier can be approximated as Av= -RC/RE1 (refer to the manual calculations to understand where this came from). We know that we want the gain to be 50, but let’s actually choose the max possible value for the gain permitted and set it to AV=55. This is going to help in accounting for whatever gain is lost when we add the emitter follower to the CE amplifier because even though the gain of the emitter follower should ideally be 1, this is not the case in actual applications and there’s always a dip in the gain. So we know that Av=55  and we just selected RC = 27kΩ, so RE1 can be determined using simple algebra, resulting in RE1=490Ω.
 
Quiescent Collector Current and Voltage Calculations 

Now that we have the RC and RE1 values, we can calculate the quiescent collector current, ICQ, and the voltage, VCEQ. This is very important because we need to obtain the maximum possible output swing in order to meet the 8v p-p no-load requirement, which is only possible at the quiescent point of operation. The quiescent current and voltage is roughly half of the saturation current and source voltage, respectively. Since the source voltage is given to us, the quiescent voltage is simply half of that value, which results in VCEQ= 5v. To find the saturation current, IS, we can perform KVL on the output portion of the transistor and if we look at the load line diagram shown in Figure 4, the saturation current is the y-intercept of the graph, which means that we can set VCE=0. Doing this, we derive the equation IS= VCC/(RC+RE1), which means that the quiescent collector current is ICQ= 0.5IS = Vcc/2(RC+RE1). By plugging in all the known values into the equation, we determined that ICQ= 0.1818 mA or 181.8 µA.

![Screenshot 2024-10-04 002155](https://github.com/user-attachments/assets/4313c761-f862-4f56-872f-61b7e216ad7e)


What's a Push-Pull Amplifier? Why didn't we just use a Simple Emitter Follower?

It may seem tempting to use only an npn-based emitter follower as the second stage of the amplifier design. It's simple to design, it has the useful properties of having a near unity voltage gain for both loaded and unloaded cases, as well as a very high input impedence which makes it a great buffer to previous stages of the amplifier. It essentially stabilizes and keeps the output of the common emitter consistent between loaded and unloaded cases, while mitigating the loss of voltage swing thanks to it's high input impedence (check out the manual calculations document to see how the high input impedence of emitter follower preserves the gain of the CE amplifier). However, there's one major flaw when it comes to using npn emitter followers as buffers to previous amplifiers, and it's that they could only operate for positive values of the base voltages. What do I mean by this? Well one of the fundamental conditions for a BJT to even be on is for the base-emitter voltage to atleast be greater than 0.7 volts, Vbe >= 0.7 V. This means that as the value of the base voltage fluctuates, the emitter voltage will always follow it from a 0.7 V difference. 

However, since both base voltage and emitter voltage are measured with respect to a ground voltage reference, neither could drop below 0 volts. A helpful comparison would be to think of how heights and potential energy in a physics context are a similar analogy to how voltage works in circuit design. Typically, you measure the height of an object with respect to some reference height level, like sea level, and using that height, you could assess the potential energy of the object. Of course, we know that with increased height, we have increased potential energy and conversely, the closer you get to the ground, the less potential energy you have. Suppose we had a ball and we continued to drop it at progressively lower heights. Intuitively, you know that the ball will bounce less high the further you decrease the drop height. When the ball has a height of zero, it is now at the same height as our reference height, so naturally, the ball isnt going to do anything because it is no longer in the air! The same goes on in an npn transistor, where if you decrease the value of the base emitter voltage to below 0.7 volts, there won't be sufficient kinetic energy for the electrons from the n-type emitter side to cross the depletion zone into the collector side, essentially cutting off all current across the transistor and effectively turning it off. Hence, if you were to try and plot the output voltage waveform of the npn emitter follower, you would actually only get the postive half of the total sinusoidal waveform, with the rest being clipped off as the base voltage is bounded by 0v ground value and thus, breaking the Vbe >= 0.7v condition for operation.

![Screenshot 2024-10-04 033121](https://github.com/user-attachments/assets/fd2bd590-3d6a-4bf4-b02b-f2cb9d73d687)

Okay so clearly we now see that the npn emitter follower alone can't provide us with the full sinusoidal output waveform we want, so we have to think up of some solutions. A naive approach would be to think that we should simply remove the ground nodes connected to the base and emitter in the emitter follower and replace them with a negative voltage rail. Though this would indeed allow for the base voltage and the emitter voltages to now drop below the 0v limit, but it would likely disrupt the biasing of the transistor. In a standard NPN emitter follower, the biasing is designed with respect to ground (0V). By replacing ground with a negative supply, the entire biasing and operating point of the transistor may have been thrown off. The transistor might no longer operate in its active region across the full input range, leading to a distorted waveform.


