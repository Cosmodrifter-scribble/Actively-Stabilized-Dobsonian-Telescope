# Actively-Stabilized-Dobsonian-Telescope
A Newtonian reflecting telescope built from scratch with a Dobsonian alt-azimuth mount. After manual pointing, an onboard PCB-based closed-loop system stabilizes tracking by correcting for Earth's rotation, vibrations, and mechanical drift using motors — no star database, pure precision control engineering.

#Understanding how modern telescopes work  

so here we are starting this project             
today i tried to understand how modern telescopes works which are available in the market. I looked at different types of telescopes like especially the dobsonian and motorized telescopes, and studied how they point at objects and keep them in view as the Earth rotates i learned that many modern telescopes use motors, sensors, and software to track objects rather than relying only on manual movement i also explored how advanced features like GoTo systems and star alignment work at a high level, without going into heavy astronomy math. After that, I started thinking about how i can make  simpler version of these systems and can be built from scratch using basic electronics and control logic.
and then i get very tired so i stopped working ![Telescope-Parts-1-scaled](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6OTgzMzcsInB1ciI6ImJsb2JfaWQifX0=--19fc777c99745539365657aaa2a7ffc888b068ff/Telescope-Parts-1-scaled.jpg)

#Learning about microcontrollers for control systems

I am new to this, so what I did was try to figure out which type of microcontroller I should use, because there are so many available in the market. To make the decision easier, I created an Excel sheet to compare different controllers and see which one would suit this project best.

And we found the winner 

and the winner is 

ESP32-S3 WROOM-1U.

After that, I started learning about the other components I would need, such as the stepper motor and the finder scope.![Screenshot 2026-02-05 022229](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6OTgzNzIsInB1ciI6ImJsb2JfaWQifX0=--7db7b22198b1147a24338bd534d7f4014c7904be/Screenshot%202026-02-05%20022229.png)

#learning about the other parts of essential telescope 

i completed the work of finding the other essential part which are left for the telescope controller like the stepper motor and sensors which are going to help me and other things then i made my final list after comparing other type to parts and comparing to them  which are going to need to make my custom PCB
![Screenshot 2026-02-05 024024](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6OTgzODksInB1ciI6ImJsb2JfaWQifX0=--b08acb75c9a42e652ffede9e19a7c136ff18f276/Screenshot%202026-02-05%20024024.png)

#learning ho to use 3d modeling 

since i am new this so i don't know to draw or model in **onshape** so learn how to make shapes and things in it by a youtuber name CAD CAM TUTORIAL BY MAHTABALAM [very good in this thing]
and i got successful in it![Screenshot 2026-02-05 025011](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6OTg0MjcsInB1ciI6ImJsb2JfaWQifX0=--103bc1ae2895d5fc7dc459f301c4241e73326deb/Screenshot%202026-02-05%20025011.png)

#started designing my telescope body

When I sat down to design the telescope body, I didn't realize how many directions I could go. There are so many different telescope designs out there, and I had to actually pick one. So I started brainstorming.
My first idea was to go with an equatorial mount — the kind that aligns with Earth's rotation axis. It looks really cool and professional. I sketched one out with a tripod stand, imagining how it would all come together. But honestly? When I looked at the drawing, something felt off. It was bulky, complicated, and for a first build, it just seemed like overkill. The tripod geometry alone was giving me a headache. So I scrapped it.
Then I moved on to a table-mounted telescope with Alt-Az movement — Alt-Az meaning it rotates on two axes: altitude (up and down) and azimuth (left and right). I sketched this one out too, and immediately it just clicked. It's clean, it's practical, and the movement makes total sense for the kind of tracking system I'm building with the ESP32-S3. No complicated polar alignment, no weird tripod math — just two axes, two motors, done.
I'm really happy with this direction. It sits flat on a table, it's stable, and the Alt-Az mechanism fits perfectly with the stepper motors I've already selected.
So the design is locked. What's left now is to take this sketch into OnShape and turn it into an actual 3D model. I need to figure out the exact dimensions — tube length, base diameter, motor mounting points — before I can start modeling properly.
![image](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDI4LCJwdXIiOiJibG9iX2lkIn19--5fa590ab9e61411484f11e51888c0374692c8808/image.png)

#Mirror hunt

So after locking in the Alt-Az table mount design, I moved on to what I thought would be a simple task — finding the primary mirror for the telescope. Spoiler: it was not simple.
The primary mirror is literally the most important part of a reflector telescope. Everything depends on it — the focal length, the aperture, the image quality. So I started researching what kind of mirror I need. I'm looking at a parabolic concave mirror, which is the standard for Newtonian reflectors. The shape has to be mathematically precise — even tiny surface errors ruin the image.
And then came the real problem.
I'm in India.
Finding quality telescope mirrors here is genuinely hard. Most of the good optics manufacturers are based in the US or Europe — companies like Orion, Celestron, GSO. Getting one shipped internationally means dealing with customs, insane shipping costs, and the very real risk of the mirror getting damaged in transit. Not ideal.
So I started looking locally. I checked Indian astronomy forums, some Indiamart listings, a few optics suppliers. There are some vendors, but the quality is either unverified or the mirrors are meant for industrial use, not astronomy. Getting a properly figured and polished parabolic mirror with a good focal ratio — like f/6 or f/8 — is a different story entirely.
I also learned a lot today about mirror specs — things like focal length, focal ratio, and how they affect what you can actually see. A shorter focal ratio gives you wider views, longer gives you more magnification. This actually affects my whole tube length and body design too.
So the mirror search continues.

![image](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDEyLCJwdXIiOiJibG9iX2lkIn19--cd276eef2fa6c7f4d9886d3c5f3757454fc7d5d5/image.png)

# movement mechanism 

today was a really satisfying day. I finally sat down and figured out the movement mechanism of the telescope — like, actually how it's going to physically move and what's driving it.
So the design is Alt-Az, which I locked in earlier. But I hadn't actually thought through how the motors connect to the telescope body and make it move smoothly. That was today's mission.
For the azimuth axis (left-right rotation), I'm thinking of a rotating base — like a lazy susan style platform — driven by a stepper motor through a gear or pulley system. The motor won't connect directly to the tube, it'll drive the base, which keeps things stable and gives better torque control.
For the altitude axis (up-down tilt), the motor will connect to the side of the tube assembly using a simple pivot mechanism. Again, not direct drive — I want some gear reduction so the movement is smooth and precise, not jerky. Stepper motors are great for this because they move in exact steps, which means I can calculate exactly how many steps = how many degrees of movement.
Getting this right matters a lot for tracking. When I eventually implement the tracking logic on the ESP32-S3, it needs to send precise commands to both axes simultaneously to follow a star as Earth rotates.
![WhatsApp Image 2026-03-31 at 5.01.00 PM](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDY0LCJwdXIiOiJibG9iX2lkIn19--a187fca23e551cf42b4039029de50621623f2c42/WhatsApp%20Image%202026-03-31%20at%205.01.00%20PM.jpeg)
![WhatsApp Image 2026-03-31 at 5.01.23 PM](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDY1LCJwdXIiOiJibG9iX2lkIn19--3a7fc03cfc800546c17c8ada7831960c8ec38fb2/WhatsApp%20Image%202026-03-31%20at%205.01.23%20PM.jpeg)
![WhatsApp Image 2026-03-31 at 5.00.17 PM](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDY3LCJwdXIiOiJibG9iX2lkIn19--8b2f46fb0c408fac76a91e1ee1d698ba2d1c20ef/WhatsApp%20Image%202026-03-31%20at%205.00.17%20PM.jpeg)
![WhatsApp Image 2026-03-31 at 5.00.40 PM](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDY2LCJwdXIiOiJibG9iX2lkIn19--95823bf9c16e3b213dbdee8dba95e3411c935e34/WhatsApp%20Image%202026-03-31%20at%205.00.40%20PM.jpeg)

#Base complete

Today I finally started properly building the 3D model in OnShape.
After locking in the Alt-Az design and sketching out the rough movement mechanism, it was time to stop drawing and start actually modeling. And honestly — harder than I expected.
The first challenge was just figuring out the workflow. Even though I'd learned the basics of OnShape earlier, actually designing a real part with real constraints is a different thing entirely. I had to think about dimensions, tolerances, how pieces fit together — not just "make a box."
Today's focus was the base. The base is the foundation of everything — it needs to be solid, properly sized to support the tube assembly, and designed with the azimuth motor mount in mind. Getting the geometry right took way longer than I thought. Lots of trial and error, a few moments of "why is this not working," and eventually — it came together.
It's just the base for now, but it's a real part. Modeled properly

Note : Once I finalize the mirror size, I'll need to come back and adjust the base dimensions accordingly — the tube diameter and overall proportions depend directly on the mirror's aperture. Don't forget this.

![image](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDI5LCJwdXIiOiJibG9iX2lkIn19--3dcefcabfe100b8b63304de218ccf112199d7386/image.png)

#The Tube Takes Shape

Today I moved on from the base and started modeling the telescope tube and its components in OnShape.
The tube is basically the heart of the optical system — it holds the primary mirror at the bottom, the secondary mirror at the top, and the focuser on the side. Getting the proportions right matters because everything inside has to align perfectly on the optical axis. Even a small offset ruins the image.
I started with the main tube body — a hollow cylinder. Then I worked on the mirror cell at the bottom, which is where the primary mirror will eventually sit. After that I roughed out the top ring, which will hold the secondary mirror spider assembly.
It's still rough — no focuser modeled yet, no proper mounting holes — but the basic tube structure is there in 3D.
Honestly the hardest part was keeping everything centered and symmetrical in OnShape. Small mistakes in sketches snowball fast when you're building a cylinder around an optical axis.
![image](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDQxLCJwdXIiOiJibG9iX2lkIn19--5309f52d61dfe1f9a27b7ab4256cf158972d1c29/image.png)

#Circuit Rough Draft

Today I shifted gears from mechanical to electrical — started figuring out the circuit for my custom PCB.
The goal was to map out a rough schematic of how everything connects. At the center of it all is the ESP32-S3, and around it I need to connect the stepper motors, their drivers, power supply, and any sensors. Getting this all to talk to each other correctly — that's the puzzle.
I started by listing every component that needs to be on the board and what pins they need. Stepper motor drivers need step and direction signals from the ESP32. Power needs to be regulated properly — the motors run at a different voltage than the microcontroller. That means separate power rails, which adds complexity.
Drew out a rough schematic — nothing clean or final, just boxes and lines to understand the flow. Already spotted a few things I hadn't thought about, like decoupling capacitors and where to put the motor driver ICs relative to the ESP32.
It's messy right now. But that's the point of a rough draft — figure out the problems before they end up on an actual PCB

![WhatsApp Image 2026-03-31 at 5.02.20 PM](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDkwLCJwdXIiOiJibG9iX2lkIn19--e8f0ef4ac78b144f4d28c9fe5fe2a4c8cc44fbb4/WhatsApp%20Image%202026-03-31%20at%205.02.20%20PM.jpeg)

#The pcb

9 hours. 9 whole hours.
I spent today learning PCB design properly and actually trying to lay out the board. Routing traces, placing components, figuring out clearances — it is genuinely hard. Like, a different level of hard than anything else in this project so far.
I was going deep into it. Learning design rules, understanding trace widths for power vs signal lines, figuring out how to handle the motor driver heat dissipation. It was a lot.
And then after all of that — I realized we're not making a custom PCB.
Not because I gave up. But because when I actually sat down and thought it through, it doesn't make sense for this project right now. Custom PCBs take time to manufacture, cost money to get wrong, and add a whole layer of complexity that could delay everything else. There are existing motor driver modules and breakout boards that can do exactly what I need — cleaner, faster, and way less risky.
Sometimes the smart move is knowing what not to build.
9 hours of learning PCB design wasn't wasted though — I understand circuits way better now than I did this morning

![Screenshot 2026-02-05 025011](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6OTg0NTEsInB1ciI6ImJsb2JfaWQifX0=--375fd0b000c4190fa776fdb410c47e0d3a178fa8/Screenshot%202026-02-05%20025011.png)

#Wiring & Unexpected Additions

Started wiring today and somehow ended up with more components than I began with.
That's just how it goes I guess.
As I was laying out the wiring plan, I started thinking more carefully about how the telescope will actually know where it's pointing. And that opened a rabbit hole. For proper GoTo functionality and star alignment, I need the telescope to know two things — its location on Earth and its current orientation.
So I added two new components.
First — a GPS module. This will give the ESP32-S3 real time coordinates and time data, which is essential for calculating where any star should be in the sky at any given moment. Without location data, the whole tracking math falls apart.
Second — a magnetic encoder. This will track the actual angular position of the telescope axes. Stepper motors are great but they don't give feedback — if a step gets missed, the telescope doesn't know. The encoder fixes that by telling the controller exactly where the tube is pointing.
Both of these make the system significantly smarter.
Got halfway through integrating them into the wiring plan before calling it for today. Still need to finalize the encoder placement and figure out the GPS antenna routing.

![Screenshot 2026-02-05 025011](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6OTg0NTEsInB1ciI6ImJsb2JfaWQifX0=--375fd0b000c4190fa776fdb410c47e0d3a178fa8/Screenshot%202026-02-05%20025011.png)

#Wiring Done, Model Updated

Finished the second half today. Feels good.
Completed the GPS and magnetic encoder integration into the wiring plan. Everything is mapped out now — ESP32-S3, stepper drivers, GPS module, encoders on both axes, power rails. The full picture is finally on paper and it actually makes sense. No major conflicts, connections are clean.
After wrapping up the wiring I jumped back into OnShape to make some adjustments to the 3D model. Adding the GPS and encoder meant I needed to think about where these components physically sit on the telescope. The encoder placement especially affected the design — it needs to mount right at the rotation axis to accurately read the position, so I had to modify some parts of the base and altitude pivot to accommodate it.
Small changes but important ones. This is exactly why I left notes earlier about coming back to adjust things — components always affect the physical design in ways you don't expect until the wiring is done.
The model is still not final but it's significantly more accurate now than it was before.

![Screenshot 2026-02-05 025011](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6OTg0NTEsInB1ciI6ImJsb2JfaWQifX0=--375fd0b000c4190fa776fdb410c47e0d3a178fa8/Screenshot%202026-02-05%20025011.png)

#finale completed

 I've been completely off the radar for a while because exams hit hard and I had zero time to sit down and write anything. But the project didn't stop — actually a lot happened, and I want to get it all down properly.
So let me tell the story from where I left off.

After finishing the rough 3D model, I kept staring at it and something felt incomplete. The more I looked at it, the more I started noticing gaps. The first thing that bothered me was precision — like, stepper motors are great, but if a motor skips a step (which they do, especially under load), you have no way of knowing. The telescope just silently points at the wrong place and you have no idea. That's when I realized I needed encoders on both axes. Encoders measure the actual physical position of the axis, not just what the motor thinks it did. So if a step gets missed, the encoder catches it and corrects it. It changes the whole system from open-loop (blind faith) to closed-loop (verified movement). Once I understood that, it felt non-negotiable.

The other thing I changed was the stand design. I originally had this single-arm pivot idea, but it felt wobbly just thinking about it. A telescope tube is not light, and when you're trying to hold a precise position while tracking a star, any flex in the mount is a disaster. So I redesigned it to have two side arms — a proper rocker-box style cradle — so the tube sits between two support points on the altitude axis. Way more rigid, way more stable.

There were other small changes too — motor placement, cable routing, thinking about where the PCB would actually live inside the structure. Every time I thought I was close to finalizing the design, I'd find one more thing to rethink. Honestly that's just how building goes.

Then came the mirror situation. And wow, what a journey that was.
I knew from the start that the primary mirror is the heart of the telescope. Everything else can be iterated — electronics, software, mechanics — but the mirror is the one thing you cannot compromise on. So I started searching.

India is genuinely hard for this stuff. Most of the good optics come from the US or China, and the well-known manufacturers like GSO, Orion, and Celestron either don't ship here directly or the import duty makes it painful. And then there's the Aliexpress situation — it's banned in India, so that entire ecosystem of cheap optics and components that builders in other countries rely on is just... not available to me. I had to get creative.

I spent a lot of time digging through forums, astronomy groups, and random e-commerce listings. Most of what I found in India was either too small (4" or less), had no specs worth trusting, or was priced like it was made of gold. At one point I found a 6" f/5 mirror on Alibaba for around ₹11,000 plus shipping — and I seriously considered it. But then I kept looking.
And then I found it.

On an Indian astronomy seller's page, there was a GSO 8" f/5 parabolic mirror — professional grade, proper specs — originally listed at ₹26,000. But it was on offer for ₹13,000. I double checked. I refreshed the page. Still ₹13,000. I did not think for long. That was the one.

Going from a 6" to an 8" mirror is not a small upgrade. The aperture goes from 150mm to 203mm, which means the mirror is collecting significantly more light — we're talking 837 times more light than the naked eye. The limiting magnitude jumps to around 14.5, meaning I can see objects 1600 times fainter than what's visible without a telescope. Deep sky objects — nebulae, galaxies, star clusters — all become actually reachable. It completely changed what this telescope is capable of.

But of course, going from a 6" to an 8" mirror meant one thing: I had to redesign basically everything.
The tube diameter, the tube length, the rocker box width, the mirror cell, the secondary mirror size, the focuser position — all of it is calculated around the primary mirror. So I went back to OnShape and started over on the body. New dimensions, new proportions, new everything. It was tedious but honestly it was also kind of satisfying because I understood why every dimension was what it was.
And while redesigning, I hit another India problem — the focuser.

A GSO 2" Crayford focuser is the standard recommendation for this kind of build. Smooth, no image shift, works great. But when I looked it up on Indian stores, the prices were just painful — heavily marked up, sometimes not even available for direct shipping. So I thought, fine, I'll just build one myself. How hard can it be.

Turns out, pretty hard. My first attempt at making a Crayford focuser from scratch was... not good 

so what i did any sane person should do copy paste

I found this build log by Marcos ATM — a beautifully documented 2" Crayford focuser made from scratch — and instead of trying to reinvent it, I just used his design as the base. Studied it, understood how the geometry worked, and made some tweaks to fit my tube dimensions. That approach worked. Sometimes the smartest move is to stand on someone else's shoulders rather than start from zero.

Meanwhile I was also trying to make the PCB, which was its own separate adventure.
My first attempt was a disaster. I had the schematic in my head, I had KiCad open, and I thought it would be straightforward. It was not. The first board I designed had routing errors, wrong footprints, and a power trace that was embarrassingly thin for what it needed to carry. Humbling doesn't even cover it.
But I didn't give up 

**I should have** 

I came back with a completely new plan. Instead of soldering everything directly onto the board and creating one giant unserviceable brick, I decided to make the key modules removable. The ESP32-S3 sits in a socket — if it fries or needs to be reprogrammed separately, I just pull it out. Same goes for the GPS module, the IMU, the motor drivers. Everything critical is modular. This also means if I want to upgrade a component later, I'm not throwing away the whole board.

this what we called big brain

It was a better approach in every way, and honestly I should have thought of it the first time. The second design came together cleanly — proper footprints, good power traces, decoupling capacitors in the right places, everything labeled. First PCB I've actually been proud of.

And then I checked my total spending.[after the full build]

I had a $400 budget for this whole project. I was being careful, comparing prices, making smart choices. And then I sat down one evening and actually added everything up — mirrors, motors, drivers, PCB fabrication, PVC, plywood, fasteners, the focuser parts, the GPS module, encoders, the OLED, shipping costs that sneak up on you — and I had crossed $400 without even realizing it.

It just... happens. You're not buying one expensive thing, you're buying forty small things, and somewhere in the middle you stop doing the mental math. By the time I noticed, I was already over. No dramatic moment, no single bad decision 

not that much of a big brain

just the quiet creep of a project that kept getting better and therefore kept getting slightly more expensive.

It is what it is. The telescope is worth it.

So here's where everything stands and what this telescope actually is now:
This is an 8" f/5 GPS-controlled motorized Dobsonian, built completely from scratch. The optical system is centered around a GSO 203mm parabolic primary mirror with 1/8 wave accuracy and a GSO 50mm elliptical secondary with 1/12 wave flatness — professional grade optics. The focuser is a custom-built 2" Crayford design, tweaked from Marcos ATM's open build.

The structure is a 9" PVC tube sitting in a 12mm plywood rocker box with two-arm altitude support. The azimuth axis uses a custom 360-tooth gear ring split into 6 wooden segments — 360 teeth means 0.003° accuracy per step, better than most commercial GoTo mounts. The altitude axis uses a 120T GT2 belt drive. Both axes driven by NEMA17 stepper motors through TMC2209 drivers at 32× microstepping — virtually silent, zero vibration.

The brain is an ESP32-S3 dual-core microcontroller in a removable socket on a custom PCB, alongside socketed GPS, IMU, and driver modules. A NEO-M8N GPS gets location and time on boot — no manual alignment needed. A MPU9250 IMU handles active stabilization at 200Hz. Two 1000PPR optical encoders close the control loop on both axes. The interface is a D-pad, OLED display, and Stellarium over WiFi — click any object on your phone, telescope points there.

Sidereal tracking, closed-loop correction, active stabilization, astrophotography capable. Total cost: somewhere just north of $400.

A Celestron NexStar 8" costs +$2000, has none of these features, and you'd never understand how it works. This one I built myself, and I understand every single part of it.
That's the telescope

![image](/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNDg4LCJwdXIiOiJibG9iX2lkIn19--9aed2af59082cd4fda0f09125dbb459e44e94cd1/image.png)




