# loadbalancing_practice

This code is originally from https://github.com/nerds-ufes/p4-learning and I give full credit to the copyright holder.

I made slight change into exercises/10-Congestion_Aware_Load_Balancing.
pre-installations about these codes can be hassle.
This code runs with old p4util code and python 2.7
I recommend you to download OVA package from https://drive.google.com/file/d/1tubqk0PGIbX759tIzJGXqex08igFfzpD/view.

upgrading the code to work with python 3 is planned.

This code is about loadbalancing sheme advanced than ECMP(equal-cost multi-path).

# why not ECMP?
ECMP does not take into account that the size of flows varies greatly (typically, there are a few very big flows and many small flows). Even if flows are uniformly distributed over many paths, this can lead to problems if many big flows are concentrated on the same path. Addressing this issue -- which can lead to congestion -- is the goal of this code.

likewise in the above github code p4-learning, flowlet hashing is used and congestion avoidance is implemented through
reading standard metadata enq_qdepth.


# installation

Before you start this code, 

You will have to install two new tools :

1. 'nload`: a console application which monitors network traffic and bandwidth usage in real time.

   ` sudo apt-get install nload`

2. `iperf3`: the newer version of `iperf` that provides new options:

    `cd /tmp`
    `sudo apt-get remove iperf3 libiperf0`
    `wget https://iperf.fr/download/ubuntu/libiperf0_3.1.3-1_amd64.deb`
    `wget https://iperf.fr/download/ubuntu/iperf3_3.1.3-1_amd64.deb`
    `sudo dpkg -i libiperf0_3.1.3-1_amd64.deb iperf3_3.1.3-1_amd64.deb`
    `rm libiperf0_3.1.3-1_amd64.deb iperf3_3.1.3-1_amd64.deb`

For this exercise we provide you with the following files:

- p4app-line.json: describes the topology we want to create with the help of mininet and p4-utils package. This linear topology will be used for our first test.
- p4app.json: describes the topology we used in the ecmp exercise, 2 switches connected through 4 middle switches.
- p4src/loadbalancer.p4: we will use the solution of the 05-Simple_Routing exercise as starting point.
- send.py: a small python script to generate tcp probe packets to read queues.
- receive.py: a small python script to receive tcp probes that include a telemetry header.
- routing-controller.py: routing controller of the 05-Simple_Routing exercise as starting point.
- nload_tmux_*.sh: scripts that will create a tmux window, and nload in different panes.
- send_traffic_*.py: python scripts that use iperf3 to automatically generate flows to test our solution.



First, clone the repository

`git clone https://github.com/kyeo95/loadbalancing_practice.git`

and run the code to configure networks and links

`sudo p4run`


If you want to see traffics goes by, open another terminal

`tmux`

and type  

`./nload_tmux.sh`

with another terminal

`sudo python send_traffic.py 20`

you can make possibly large traffic sent from hosts.
remember send_traffic.py <duration time>.
