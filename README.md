# Formal Verification of a SCUBA Dive Computer

This repository contains formal methods associated with our EMSOFT 2019 paper [Verifiably safe SCUBA diving using commodity sensors: work-in-progress](https://dl.acm.org/doi/10.1145/3349568.3351554)

[SCUBA diving](https://en.wikipedia.org/wiki/Scuba_diving) is an activity in which divers remain underwater for prolonged periods by using a self-contained breathing apparatus. Diving is safety critical because changing depth too rapidly or running out of oxygen before surfacing can result in [life-threatening consequences](https://en.wikipedia.org/wiki/Decompression_sickness). These risks are currently minimized by using a wrist-mounted, 'air-integrated' dive computer that monitors time, depth and air tank pressure (received through expensive wireless transceivers or a hose). These computers are costly for the average recreational dive. 

We present a [hybrid systems model](https://en.wikipedia.org/wiki/Hybrid_system) and safety proof for a SCUBA diving computer that estimates air consumption of the diver using commodity heart rate sensors. We employ a mathematical model of oxygen uptake in response to exercise and thereby predict the time remaining for the air supply to be depleted, as well as obviate the need for a tank-mounted wireless transmitter or the cumbersome hose integrated computer for directly monitoring tank pressure. We formally verify a controller that ensures the diver can always surface without running out of breathable air.

Our model is written in differential dynamic logic and our proofs are constructed using [KeYmaera X](https://keymaerax.org) with the Mathematica backend for first order real arithmetic.

# Repository Structure

There are three models/proofs:

 * tApprox: Contains the theorem statement, proof, and proof documentation for the basic model.
 * tApproxUpdate: Allows occasional manual ground truth updates for `t`.
 * nitrogen: Establishes both nitrogen safety and oxygen safety.

Each folder contains:

 * a `.kyx` file: A human-readable copy of the KeYmaera X model (open in your favorite text editor that supports unix line endings).
 * a `.kyt` file: The [Bellerophon](http://nfulton.org/papers/bellerophon.pdf) tactic for proving the property in the `.kyx` file.
 * a `.kya` file : A KeYmaera X "archive" file containing the model and proof.

## Instructions for Checking the Proofs

### Part 1: Download, install, and initialize KeYmaera X

1. Install the correct version of the Java runtime. 
You will need a Java 8 JRE. On Ubuntu, run `sudo apt-get install openjdk-8-jre` then run 
`sudo update-alternatives --config java` and select the version 8 option.

2. download [version 4.4 of KeYmaera X](https://github.com/LS-Lab/KeYmaeraX-release/releases/download/4.4/keymaerax.jar)  (*note: this is NOT the current version!*)

3. If you have a previous version of KeYmaera X installed, back up and then delete your `~/.keymaerax` directory.

4. Now we need to accept the KeYmaera X lincense (GPL) and initialize the KeYmaera X lemma cache.  To do this, boot KeYmaera X by running:

    java -jar keymaerax.jar

Once KeYmaera X is running, a browser window should open to `http://127.0.0.1:8090/`. 
If this does not happen automatically after a couple of minutes, navigate your browser to that URL manually.

5. Register an  account and accept the license.

6. If you have a Mathematica installation, setup KeYmaera X to use mathematica by going to `http://127.0.0.1:8090/dashboard.html#/tools`, 
selecting `Mathematica` as the `Arithmetic Solver`, and then selected defaults.

*NOTE*: Mathematica is optional for KeYmaera X, but our proof may not complete without a mathematica installation 
because Z3 will time out on some subgoals and some machines. If the proof does not complete during part 2 and you see a warning message about Z3 in STDOUT, then you will need to install Mathematica and configure KeYmaera X to use mathematica before proceeding.

7. While logged in to KeYmaera X, click the "shutdown / turn  off" icon (NOT the "log out" icon). This is the second to last icon on the right-hand side of the menu  bar. You should see a " KeYmaera X is now offline. Please close the browser window." message.

### Part 2: check the proof

1. copy the archive file (`.kya`) into the directory that your keymaerax.jar file is located in.

2. Once KeYmaera X is shut down, run:

    java -jar keymaerax.jar -check final.kya

The final command might take a while, especially if you don't have a Mathematica installation. Warning: this will drain a laptop's battery.

You should see the following output:

      ==========================================
      Proving final.kya: model nitrogen with tactic nitrogen: Proof
      =============== Succeeded ===============
      Proved final.kya: nitrogen with nitrogen: Proof
      Proof duration [ms]: 92782
      QE duration [ms]: 78803
      Tactic size: 347
      Tactic lines: 228
      Proof steps: 63687
      ==========================================
      =============== Succeeded ===============
      Proved final.kya: nitrogen with nitrogen: Proof
      Proof duration [ms]: 92782
      QE duration [ms]: 78803
      Tactic size: 347
      Tactic lines: 228
      Proof steps: 63687
      ==========================================
      Shutting down Mathematica...
      ...Done
      No need to shut down MathKernel if no link has been initialized

You can check each of these proofs by changing into the appropriate directory and running KeYmaera X
with the -check flag on the appropriate .kya file. For example,

    cd nitrogen;
    java -jar path/to/keymaerax4.4.jar -check final.kya

# Contact

 * Viren Bajaj
 * Karim Elmaaroufi
 * Nathan Fulton <nathan@nfulton.org>
