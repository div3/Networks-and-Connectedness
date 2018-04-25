# Model Proposal for Networks and Connectedness

Divyansh Saini

* Course ID: CMPLXSYS 530,
* Course Title: Computer Modeling of Complex Systems
* Term: Winter, 2018


 __*LS COMMENTS:*__
 *Good use of the course project and I think you will likely be able to find some interesting things. You currently have several elements you are considering all at once that you haven't gotten around to implementing via code. I actually might suggest in the immediacy, simplifying your model some to only look at say, the different types of nodes OR bias and importance in information transmission first and then building up from there.*


&nbsp; 

### Goal 
*****

The goal of this model is to study how complexity of connections in networks affects their ability to transmit information. 
This so-named 'complexity of connections' forms the main crux of this project and can be split 4-ways into the complexities regarding:
 - The relative connectedness of each node,
 - The varying kinds of nodes - producers, consumers, transmistters
 - The relative numbers of each kind of node (Redundency analysis) 
 - How well different stratergies are able to maximize information traversal.
 
 __*LS COMMENTS:*__
 *I am interested in understanding better what you are referring to as "strategies" here. I think I can pick up some of it from the below, but with bias vs. importance along with agent type, there are a lot of moving parts and I can't quite tell how they are all interacting here. Either clarifying that more explicitly or simplifying the model first and building up would be helpful.*

&nbsp;  
### Justification
****
An Agent Based Modelling(ABM) approach here is justified because the nodes this project studies can be concieved of as agents, or decision making entities. This is apparent through the following points - 
 - Their behaviour is dependent on making individual decisions in order to optimize some pre-defined problem, in this case that is information traversal. 
 - There exist varying kinds of nodes, each of which have their own seperate goals, and even though they are initalized to similar states, they quickly become individualistic and hence,
 - the nature of interactions between any two different nodes is individulistic to those two nodes. 
 
As these qualities are very closely associated with 'agents' in an ABM, it is apparent that an Agent based model centered around these nodes as agents is a justified way to approach this problem.

&nbsp; 
### Main Micro-level Processes and Macro-level Dynamics of Interest
****

This model will be constructed of producer, transmitter and consumer nodes, with varying priorities assigned to each node _and_ strength to each connection.

The key processes that this model is designed to explore are -
 - How/When/If such a system can reach equilibrium with respect to the strength of its connection between nodes. 
 - How will assigning the information priority affect the system.
 - How do different optimizations in regards to increasing information traversal affect the throughput of system.

&nbsp; 


## Model Outline
****
&nbsp; 
### 1) Environment
_Description of the environment in your model. Things to specify *if they apply*:_

* Dimensionality: 2D
* List of environment-owned variables: node_list, network_bias
* List of environment-owned methods/procedures: assign_node_position, create_information_list, add_node

The enviornemnt will be represented on a 2D plane. The enviornment is responsible for generating and assigning location to 
to different nodes and creating an information vector. The information will also have some priority associated with it. 

&nbsp;
__*LS COMMENTS:*__
*Is the information coming from the environment on top of which the producer nodes are situated? Do other nodes get information directly from their environment in addition to from the network? If only producers get information from the environment, you might simplify the model by just having each producer generate its own information each turn and not go through a procedure to interact with the environment (especially since the environment isn't changing at all by virtue of the producers' actions - it's just generating signal)*

```python
class space(self):
    def __init__(x= 1000, y=1000, p_range = priority_range, network_bias = 0):
        priority_range = p_range
        x_bound = x
        y_bound = y
        bias = network_bias

    def create_information_list(self, num, value_range):
        for i in range(num):
            info = tuple(i, random.value(0, value_range))
            information_list.append(info)
        return information_list
    
    node_list = []

    priority_range = int()
    x_bound = 0
    y_bound = 0
    id = 0
    
    def add_node(node_type, restrict_x = x_bound, restrict_y = y_bound): # Restrict placement for better viewing
        position = assign_node_position(x_range, y_range)
        node = tuple(node_type, position)
        id += 1
        node_list[id] = node
        return id
        
    
    def assign_node_position(self, x_range, y_range):
        while(True):
            x = random.values(0, x_range)
            y = random.values(0, y_range)
            for node in node_list:
                if x == node[1] or y == node[2]:
                    continue
                else:
                    return(x, y)

###LS COMMENTS###
# What does "priority_range" do? Also wasn't sure what "network-bias" was doing here in the space object? Do you want that to be 
# tied to something in the environment?


```


&nbsp; 

### 2) Agents
 
 _Description of the "agents" in the system. Things to specify *if they apply*:_
 
* _List of agent-owned variables (e.g. age, heading, ID, etc.)_ : type, connections_list, id, info_at_hand
* _List of agent-owned methods/procedures (e.g. move, consume, reproduce, die, etc.)_: create_self, transmit, update_bias, recieve_info


```python
class node(self):
    def __init__(kind):
        type = kind
    bias = {}
    info_at_hand = None
    id = None
    def recieve_info(info, id):
        info_at_hand.append(info, bias[id]*info[1]) # Append info, modified with bias * info value
        bias[id] = update_bias(node, bias[id], info[1])
    def update_bias(node, bias, value):
        return
    def transmit(info, node_list):
        for node in node_list:
            recieve_info(info, id)
    def get_id():
        return id
    def set_id(value):
        id = value

# Each will have a seperate way of updating their bias. I could have implemented them all in the same function above,
# but this ensures seperation between the kinds and allows for bettwe updating in case of type specific funcitons, such
# as consumer-consumer sharing
class producer(node):
    def __init__:
        kind = 'Producer'

class consumer(node):
    def __init__:
        kind = 'Consumer'
    def consumer_consumer():
        # Consumer sharing
    def recieve_info():
    
    def update_bias():

class transmitter(node):
    def __init__:
        kind = 'Transmitter'
    consumer_list = []

##LS COMMENTS##
# Related to prior comments, wasn't sure if information is being received only through the network or through network and environment? 
# Similarly, wasn't sure about bias in this respect either. Really important to hammer out the details of each agent type ASAP to make
# sure that everything is interacting smoothly. 

```

&nbsp; 

### 3) Action and Interaction 
 
**_Interaction Topology_**

_Description of the topology of who interacts with whom in the system. Perfectly mixed? Spatial proximity? Along a network? CA neighborhood?_

 - Each producers interacts with a set number of transmitters in the system(assigned on initialization)
 - Each consumer acts with a set number of transmitters in the system.
 - But each consumer can attach to multiple other consumer, but must share information at each turn with only a set number of them.
 
**_Action Sequence_**

_What does an agent, cell, etc. do on a given turn? Provide a step-by-step description of what happens on a given turn for each part of your model_

Producers:
1. Get info from environment
2. Pass info to all connected transmitters, update information importance with bias.
3. Update the bias for each connection.

Transmitters:
1. Get info from producers.
2. Pass info to set number of connected consumers, update information importance with bias.
3. Update the bias for each connection, decrement it for consumers you did not interact with this turn.

Consumers:
1. Get info from transmitters.
2. Update the bias for each connection, decrement it for transmitters you did not interact with this turn.
3. 'Utilize information' (A hopeful extension) to align yourself on a spectrum
4. Share information with all connected consumers, update the bias. 


__*LS COMMENTS:*__
*Not sure how bias is being handled here yet, but bear in mind that your choices here will probably be the most critical for your modeling outcomes. I'd encourage you to start simple - maybe even having all agents update bias similarly and have it affect information transmission similarly - and then modify from there.

&nbsp; 
### 4) Model Parameters and Initialization

_Describe and list any global parameters you will be applying in your model._
 - Bias(int) -> on each connection, by what value is the information amplified. _Potentially_ it will average the value of information you get from each node and increment/decrement it based on if the new information you received is higher/lower than the average.
 - Segregate(bool) -> If the network is to be segregated into parts at the producer/transmitter level.
 - Connectedness between Producers/Transmitters -> How complete are the connection between the set of producers and their eligible transmitters.
 - Between Transmitters/Consumers -> How complete are the connection between the set of transmitters and their eligible consumers.
 - Consumer_consumer_connections (int) -> connectedness between consumers

__*LS COMMENTS:*__
*Not sure how the "segregate" parameter works from this description. Do you mean that there won't be overlaps in transmitters connected to different producers when it is in effect?"

_Describe how your model will be initialized_
 1. Create a space and define its bounds.
 2. Create the nodes and place them in their respective areas.
 3. Create connections between nodes given a set of parameters including bias(int), segregate(bool), connectedness between Producers/Transmitters and between Transmitters/Consumers, consumer_consumer_connections (bool) -> connectedness between consumers
 4. Initialize a vector of tuples called 'information_list' with each item containing an info_id, _id_ and a level of importance _imp_
 5. Assign a piece of info to each node according to its type.
 
_Provide a high level, step-by-step description of your schedule during each "tick" of the model_
1. Generators pass on information to transmitters, modifying the information's importance according to a relative bias b, if assigned.
2. Transmitters decide on what information to pass on to the consumers.
3. They pass on info to each node they are connected to.
4. Transmitters update their relations with Producers based on the importance of info they received.
5. Consumers update their relations with Transmitters based on the importance of info they received.
6. Consumers share info with other consumers and update their relations based on the importance of info they received.

'Update their relation' here means the value of bias that exists on each connection and the relative strength of that connection. Importance of information received is inversely proportional to the strength of connection. 

__*LS COMMENTS:*__
*Not sure how "importance" is working here per the above, especially as it interacts with bias and connection strength. What's the thinking behind it being inversely proportional to connection strength? Also, how are you capturing strength connection? Is that the "bias" or is it something different in terms of edge weight or the like?

&nbsp; 

### 5) Assessment and Outcome Measures

_What quantitative metrics and/or qualitative features will you use to assess your model outcomes?_
How long does a given piece of information 'I' stay in the system, given its importance. 
How closely does the system align itself along different runs given a certain set of initial parameters?
Why system designs are most efficient/inefficient in transmitting information (throughput of the network)?
&nbsp; 

### 6) Parameter Sweep

_What parameters are you most interested in sweeping through? What value ranges do you expect to look at for your analysis?_

The parameters that I am most interested in sweeping through are the values of 'importance' of information, the number of transmitter nodes and the relative connectedness of the connections in the consumer nodes.
