# P1: Run the Flow!

Version: 2025.0
---

## Due Date:  FIXME



# Goal

This project will walk you through the basics of a digital IC design flow using a PICORV RISC-V CPU core.  It will introduce you to both logic synthesis and place and route.  It will also help you understand the goals of optimizing Power/Performance/Area (PPA) for a digital circuit.  

# Setup

## Login to Burrow - RedHat

```bash
ssh burrow-rhel.luddy.indiana.edu -YCA
```

## Get the starter code

This only needs to be done once per project

```bash
git clone git@github.com:engr599-ic/P1_run_the_flow.git
cd P1_run_the_flow
make setup
```


# Running the flow

## Load CAD tools

This needs to be every time you log in.  It loads the Computer Aided Design (CAD) tools we'll be using.  They are also called "Electronic Design Automation (EDA)" tools.  

```bash
source load_tools.sh
```

If you get a "command not found" error, it's likely you forgot to (re)run this command. 

You can also add this to your `~/.bash_profile` if you want it to get run every time you log in.  

## Synthesis

This will run Synthesis, a process where an abstract description of a digital circuit (often at the register transfer level or RTL) is automatically translated into a gate-level implementation, optimized for specific design constraints

```bash
make synth
```

This will launch a tool named `genus`, and ask it to run the `synthesis.tcl` script.  It maps our RTL to Skywater Technology's S130 Process. This typically takes a few minutes. 

Once this is complete, it will generate a `postsynth.vg` file.  This is a Verilog Gate-Level netlist.  

## Place and Route

Place and Route (P&R or PnR) is where electronic components and their interconnections are automatically arranged and routed on a chip or printed circuit board (PCB).  It is also called Automatic Place and Route (APR).  

```bash
make pnr
```

This launches a tool named `innovus`, and asks it to run `pnr.tcl`.  This will do the P&R on our previously synthesized netlist.  Once complete, you can open the database and view your results.  

```bash
innovus -stylus -db <path to database>
```

By default all database files are saved to the `dbs/` dir

# Your Turn

Now that you know the basic flow, it's time to start tweaking for improved PPA.  Your task is customize this flow twice, once to optimize performance and once to optimize for area.  

## Optimize for Performance

By default, your PICORV core is targetting a 100ns (10MHz) clock.  Your task is to see how high you can push the frequency without causing setup/hold violations on any of the process corners.  

Start by modifying this line in the `functional.sdc` Constraint file: 
`create_clock -name clk -period 100 -waveform {0 50} [get_ports {clk}]`

You will likely need to optimize both the `synthesis.tcl` and `pnr.tcl` to achieve the highest performance. 

## Optimize for Area


