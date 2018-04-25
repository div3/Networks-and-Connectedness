# Final Model
#### Abstract
The following text discusses a model studying the affect of biases in a Producer - Transmitter - Consumer system. First, the rules of information creation, transmission and consumption are explored. Then emphasis is placed on parameter sweeps conducted with relation to the biases for each type of node in the system, followed by an analysis of the emergent systems that arise. The analysis finds that compared to other nodes biased transmitters have the most affect on a system.

## Introduction
Information Traversal is becoming increasingly complex given the rise of the digital age. An overwhelming majority of people rely on the media, or social networking sites to inform themselves of the world. An article by the Columbia Journalism review(Wilner) here presents the confirmation bias that exists in the reporting of news when the news agency itself could be hurt by presenting it. It discusses how the media is less likely to present information it does not agree with.

Fox News and Media Bias(DellaVigna, Stefano, and Ethan Kaplan) Presents finding from the affect the Fox media, a traditionally conservative organization, had on voting when it moved into certain markets. It increased the amount of people going to the polls and the people voting for the conservative party. This is a clear indication of how media can change people's inclinations.

Media Bias and Influence: Evidence from Newspaper Endorsements (Chun-Fang Chiang  Brian Knight) here presents that the consumers are more likely to support a candidate that a newspaper they trust has favored publically in an endorsment. This is more proof of the affect that the information presented to the consumers can have drastic afffects on voting in an election.

This paper models three different kinds of nodes:
- Producers, who create aligning information: Analogous to people/organization that put out original information.
- Transmitters, act as a buffer between producers and consumers : Analogous to the media
- Consumers: Who receive information: Analogous to the general population

This model is a simple representation of the world using the above defined nodes as the only entities in the system. There are various factors not considered in this initial version of the model, these include producers transmitting the information themselves, Transmitters adapting to the consumers inclinations etc. A variety of assumptions are also made regarding both inclination and bias updating.

For analysis of the bias and its affects, a spatial voting model is used, with only consumers getting to vote. Otherwise, the model also studies the similarities between consumers perceptions about the world, based on two factors: The information they have received so far, and asking other consumers that they are connected to for their perceptions. 

## Why an ABM?
An Agent Based Modelling(ABM) approach here is justified because the nodes this project studies can be concieved of as agents, or decision making entities. This is apparent through the following points -
-   Their behaviors is dependent on making individual decisions taking into account unique information that they have received, and their own unique properties.
-   There exist varying kinds of nodes, each of which have their own separate goals, and even though they are initialized to similar states, they quickly become individualistic and hence,
-   the nature of interactions between any two different nodes is individualistic to those two nodes.
- The overall emergent system in the model is not well reflected in the micro-transactions that happen in the system.
- It is easier to model, and study the probabilistic behaviors that the 'Nodes' exhibit using an Agent based Model.

As these qualities are very closely associated with 'agents' in an ABM, it is apparent that an Agent based model centered around these nodes as agents is a justified way to approach this problem.

## Model Outline

### Environment:
The environment does not have a direct affect on the nodes, agents etc. All the computation is handled by the nodes themselves.
Perhaps the initial bias could be thought of as a factor of the environment, but beyond setting the initial values for the node the environment does not factor into future analysis.

### Agents
As described above, there are three kinds of Agents, henceforth referred to as 'Nodes': Producer Nodes, Transmitter Nodes, Consumer Nodes.
There are multiple types of each node in the system. Each node has its own:
- Uniq_id: This is an identifier for each node 
- Inclination: This is a value in the range 0 - 100, this is their preference on the scale, i.e. where they stand on a particular topic, issue or general preference.
- Forward Connections: A list of uniq_id's of nodes that this particular node can transmit information to.
- Backward Connections: A list of uniq_id's of nodes that this particular node can receive  information from. For each uniq_id this list also contains the bias rating for that particular node.
	- A bias rating represents where the receiver expects the information to be in respect with their own inclination. For example, if a a node with inclination 60 has a bias rating of 0.1 for a particular sender, this means that the node expects the info to be in +/- (100*0.1) = +/-(10) of the nodes own inclination. So in the range [50, 70]
- info_list: A list of dictionaries containing the previous information that this node has received, along with what node sent it(identified by uniq_id)

Insert code

### Action and Interaction
**_Interaction Topology_**
-   Each producers interacts with a set number of transmitters in the system(assigned on initialization). This is only to transmit information.
-   Each transmitter acts with a set number of consumers in the system. This is also exclusively to transmit information
-   Each consumer can have more consumers that is is attached to. These connections are used to both transmit information, and conduct analysis.

**_Action Sequence_**
- Producers:
	1.  Generate a piece of information, which is a value randomly chosen from a normal distribution around the agents inclination. 
	2.  Pass info to all connected transmitters.

- Transmitters:
	1. Get info from producers.
	2. For each piece of information update the bias for the producer who sent it depending on where the information was on the expected scale.
	3. Aggregate all information received to create information to send to consumers, to do this:
		- First, calculate average of all info received:
			- If this is within +/-25 of own inclination, this is the information the transmitter will forward.
			- Otherwise, use own inclination as the information to forward.
	4. Send the information to all forward connections.

- Consumers:
	1. Get info from transmitters.
	2. For each piece of information update the bias for the transmitter who sent it depending on where the information was on the expected scale.
	3. Aggregate all information received to create information to send to other consumers, to do this:
		- First, calculate average of all info received:
			- If this is within +/-25 of own inclination, this is the information the transmitter will forward.
			- Otherwise, use own inclination as the information to forward.
	4. Send the information to all forward connections.
	5. Run the first three steps again to receive information from all consumers that you have a backward connection to.


### Model  Initialization
In the simplest run of the model:
-  It requires a specific number of Producers, Transmitters and Consumers to be passed in.
- As the producers are usually few in number, it spreads their inclinations evenly such that the average of all of them is ~50. 
- It then assigns each producer a random number of transmitters as forward connections.
- For the transmitters it randomally assigns them an inclination and designates each transmitter a random number of forward connections.
- For the consumers, it randomally assigns an inclination and a relatively small, random number of forward connections as consumers.
### Sample Run  
The Sample run of the model is detailed well in the actions sequence presented earlier. All runs of the model include the Producer, Trandmitters and Consumers performing all of the actions listed, in turn, every turn.

## Final Analysis
The final analysis was conducted using,
5 producers, 10 transmitters and 20 consumers. 
As this is the best representation of a small, but sufficiently varied system.
### Methods to Assess
Five different methods were used to analyze the nature of the emergent system that arose on running the model with varying initial conditions. Three of these methods analyze three different characterstics of the final model, the first 3 require the consumers to vote on a given list positions. The five methods are:
- **Spatial Model Voting**: Consumers vote for the position that has the lowest absolute distance from their own inclination. The resulting vote list is analyzed in terms of its mode and variance.
- **Vote based on recieved information**: This asks the consumers to vote based on the collective information they have recieved throughout the model run. This helps characterize the percieved inclination that the consumers deem the system has.
- **Ask Radom Neibhours**: It asks each consumer to consult a random number of other consumers and report on what the majority of them are inclined to vote for. 
- **Overall Bias**: This method aggregates the total bias all consumers have with all transmitters and analyzes it to determine the overall bias in the system and if there exists self seperation in the system.
- **Overall Inclination**: This method aggregates the total inclination all consumers determine the overall bias in the system, the deviation also helps understand if there is a significant seperation.

### Parameter Sweep
The most important part of this model is the initial inclinations that each kind of node has. To conduct a throrugh parameter sweep of the model, all different permutations of the model, with respect to three levels of 'inclination severity' were created and analyzed. To be a little more precise, all 27 combinatinos of the follwing list were generated, [Producer (High/Medium/None), Transmitter (High/Medium/None), Consumer (High/Medium/None)].
Each level of inclination severity meant that all the initial inclination for all nodes of that type were centered around:
- 15, for high 
- 30, for medium
- 50, for none

### Results
Results are grouped by the methods to assess presented earlier. As there are 27 different combinations, covering them all individually would be impractical. So, a general overview is provided, with groups coagulated together as appropriate.

#### Overall Inclination
The overall inclination for the system at the start is usually randomally distributed accroding to the setting that the model was initialized with. The next plot provides a general overview of how the inclinations change throughout the run of the model. 



In the detailed tabled placed below, Name lists the names of the types of nodes that were affected and Biases lists, what the bias was, in order. 0 is None, 1 is Low and 2 is High.

On further analysis of the model, it becomes apparent that a low bias in the transmitters is neccesary to drive the bias for the whole system up.  But by themselves they are unable to do it. In fact the system by itself seems very resilient to changing bias. It appears that it can only drive down the real inclination of the system, when the transmitters are not biased and producers are biased(Num 2, Num 12, Num 14). Producers seem to play a big role in this, which is intuitive as they are the ones creating the information. But for them to succeed, the transmitters have to be generally not randomally distributed in the system, meaninig that they should be centered around 50, rather than the initial random values from 0-100. It also becomes apparent that if the consmuers are highly biased, it is easy for the system to drive up their inclination, given that it is not a random spread.

|Num|                        Name                        |Mean |Standard Deviation|
|--:|----------------------------------------------------|----:|-----------------:|
|  0|None                                                |55.77|            7.1510|
|  1|[('producer',), (0,)]            |50.53|           28.4475|
|  2|[('producer',), (1,)]            |30.25|            0.4542|
|  3|[('producer',), (2,)]            |52.03|            0.3866|
|  4|[('transmitter',), (0,)]      |55.91|            1.8291|
|  5|[('transmitter',), (1,)]      |57.12|            4.3466|
|  6|[('transmitter',), (2,)]      |53.59|            3.7801|
|  7|[('consumer',), (0,)]            |73.45|            1.0186|
|  8|[('consumer',), (1,)]            |45.36|           12.2207|
|  9|[('consumer',), (2,)]            |36.47|            6.5529|
| 10|[('producer', 'transmitter'), (0, 1)]|48.08|           27.5011|
| 11|[('producer', 'transmitter'), (0, 2)]|46.63|           13.8876|
| 12|[('producer', 'transmitter'), (1, 0)]|39.70|           34.7188|
| 13|[('producer', 'transmitter'), (1, 2)]|51.19|            0.6057|
| 14|[('producer', 'transmitter'), (2, 0)]|35.44|           28.2826|
| 15|[('producer', 'transmitter'), (2, 1)]|42.40|           26.1644|
| 16|[('producer', 'consumer'), (0, 1)]|44.61|           26.8026|
| 17|[('producer', 'consumer'), (0, 2)]|50.83|           12.4618|
| 18|[('producer', 'consumer'), (1, 0)]|52.22|           28.3105|
| 19|[('producer', 'consumer'), (1, 2)]|51.18|            9.3706|
| 20|[('producer', 'consumer'), (2, 0)]|47.80|           30.6030|
| 21|[('producer', 'consumer'), (2, 1)]|58.91|           28.4070|
| 22|[('transmitter', 'consumer'), (0, 1)]|61.26|            2.5314|
| 23|[('transmitter', 'consumer'), (0, 2)]|49.28|           17.7104|
| 24|[('transmitter', 'consumer'), (1, 0)]|42.57|            3.3728|
| 25|[('transmitter', 'consumer'), (1, 2)]|59.56|            2.2351|
| 26|[('transmitter', 'consumer'), (2, 0)]|56.54|            1.8491|
| 27|[('transmitter', 'consumer'), (2, 1)]|61.26|            1.1356|
| 28|[('producer', 'transmitter', 'consumer'), (0, 1, 2)]|54.30|            0.5692|
| 29|[('producer', 'transmitter', 'consumer'), (0, 2, 1)]|49.40|           27.4599|
| 30|[('producer', 'transmitter', 'consumer'), (1, 0, 2)]|51.81|            0.4531|
| 31|[('producer', 'transmitter', 'consumer'), (1, 2, 0)]|61.82|           31.8813|
| 32|[('producer', 'transmitter', 'consumer'), (2, 0, 1)]|49.84|           28.4674|
| 33|[('producer', 'transmitter', 'consumer'), (2, 1, 0)]|52.02|           30.9616|

#### Overall Bias

In general there exists a high level of bias throughout the system. Here high is defined to be 0.20, as that value predicated that the reciever will ignore the value presented to them, but instead reaffirm their inclination back to themselves. 

The special cases that emerge appear in similar instances as to what was descibed above in the overall inclination section.

All interesting observations that arise of this test are listed below:
1. When the producers are placed around 50, rather than at a structured distance in the map, the consumers that exist on those extremes never change their inclination and maintain a high level of bias for the subsequent transmitters that have to relay the information created.
2. When the producers are highly biased, they are able to leverage the transmitters even spread to lure the consumers into being biased. This is probably not the case in the first observation as producers are at/around 50 where they miss the extremes that they can capture in this case.
3. The least bias in the system usually comes in the case that the consumers are not involved. But when they are, it is imperative that one of Producers, Transmitters are also biased, and the consumers are highly biased as can be seem in the cases (28, 30)
4. The Highest biases in the system comes when the transmitters are not directly biased themselves. Such is the case in 16-21. This is probably because of the aformentioned spatial distribution, wherein they reaffirm their own inclination. So if the producers, and consumers might both be biased, even if the transmitters are evenly spread there exits a high bias in the system regardless.
5. 
|Num|                        Name                        | Mean  |Standard Deviation|
|--:|----------------------------------------------------|------:|-----------------:|
|  0|None                                                |0.14436|          0.091099|
|  1|[('producer',), (0,)]            |0.33654|          0.278936|
|  2|[('producer',), (1,)]            |0.05478|          0.025669|
|  3|[('producer',), (2,)]            |0.03485|          0.002260|
|  4|[('transmitter',), (0,)]      |0.10307|          0.084460|
|  5|[('transmitter',), (1,)]      |0.16084|          0.110960|
|  6|[('transmitter',), (2,)]      |0.08612|          0.047813|
|  7|[('consumer',), (0,)]            |0.09732|          0.101630|
|  8|[('consumer',), (1,)]            |0.21231|          0.164196|
|  9|[('consumer',), (2,)]            |0.15990|          0.116354|
| 10|[('producer', 'transmitter'), (0, 1)]|0.25712|          0.265601|
| 11|[('producer', 'transmitter'), (0, 2)]|0.07526|          0.121592|
| 12|[('producer', 'transmitter'), (1, 0)]|0.28821|          0.345739|
| 13|[('producer', 'transmitter'), (1, 2)]|0.03019|          0.003118|
| 14|[('producer', 'transmitter'), (2, 0)]|0.22447|          0.269502|
| 15|[('producer', 'transmitter'), (2, 1)]|0.19958|          0.256088|
| 16|[('producer', 'consumer'), (0, 1)]|0.18378|          0.250842|
| 17|[('producer', 'consumer'), (0, 2)]|0.07143|          0.114186|
| 18|[('producer', 'consumer'), (1, 0)]|0.30645|          0.265798|
| 19|[('producer', 'consumer'), (1, 2)]|0.05547|          0.084872|
| 20|[('producer', 'consumer'), (2, 0)]|0.33894|          0.280814|
| 21|[('producer', 'consumer'), (2, 1)]|0.27287|          0.272643|
| 22|[('transmitter', 'consumer'), (0, 1)]|0.12263|          0.115592|
| 23|[('transmitter', 'consumer'), (0, 2)]|0.18558|          0.149837|
| 24|[('transmitter', 'consumer'), (1, 0)]|0.10875|          0.035333|
| 25|[('transmitter', 'consumer'), (1, 2)]|0.10459|          0.067546|
| 26|[('transmitter', 'consumer'), (2, 0)]|0.14536|          0.142869|
| 27|[('transmitter', 'consumer'), (2, 1)]|0.11282|          0.118924|
| 28|[('producer', 'transmitter', 'consumer'), (0, 1, 2)]|0.03465|          0.005525|
| 29|[('producer', 'transmitter', 'consumer'), (0, 2, 1)]|0.19928|          0.268299|
| 30|[('producer', 'transmitter', 'consumer'), (1, 0, 2)]|0.03363|          0.002375|
| 31|[('producer', 'transmitter', 'consumer'), (1, 2, 0)]|0.36885|          0.309862|
| 32|[('producer', 'transmitter', 'consumer'), (2, 0, 1)]|0.25861|          0.276353|
| 33|[('producer', 'transmitter', 'consumer'), (2, 1, 0)]|0.34782|          0.279811|

#### Spatial Model Voting
This is another way to analyze the structure of consumer inclinations. Consumers are asked to vote on two kinds of positions:
- even_spread: This is a list of 9 positions starting at 10 going upto 90, with increments of 10.
- bias_spread: This is a list - [15, 30, 50, 65, 80]

As can be seen in the table below, all the cases remain consistent with the consumer inclination method that we used to analyze the structure above. Some interesting emergance in this system are,

1. In the first case(Num 1), position 20 wins in the spread out case but position 80 wins in the more spreadout case. This is consistent with the inclinations that we have seen in the earlier methods.
2. The only time position 10 wins is when the Producers are highly biased, the Transmitters lightly biased and the consumers are evenly spread out over the system.
3. The Winner for both lists remain pretty consistent throughout the system. But the number of votes change sporadically. This is probably because of structure of the voting positions themselves.
4. The number of votes are perhaps the most interesting feature. We can see that in the traditionally seperated structres, the votes go to the mpre biases positions. It can is also the case that if the positions are too close, the votes might become divided, as is the case in the Even Winner list.
5. Number of votes in the Bias winner, present a more accurate reflection of wheather there is a bias in the system or not. As we can see the usual suspects(22, 23, 33) have either the lowest number of votes, or the highest position winner. 

|Num|                        Name                        |Even Winner|Num votes for Winner|Bias Winner|Num votes for Winner|
|--:|----------------------------------------------------|----------:|-------------------:|----------:|-------------------:|
|  0|None                                                |         60|                  11|         50|                  11|
|  1|[('producer',), (0,)]            |         20|                   8|         80|                   9|
|  2|[('producer',), (1,)]            |         30|                  20|         30|                  20|
|  3|[('producer',), (2,)]            |         50|                  20|         50|                  20|
|  4|[('transmitter',), (0,)]      |         60|                  16|         50|                  16|
|  5|[('transmitter',), (1,)]      |         60|                  18|         65|                  11|
|  6|[('transmitter',), (2,)]      |         50|                  16|         50|                  16|
|  7|[('consumer',), (0,)]            |         70|                  19|         80|                  16|
|  8|[('consumer',), (1,)]            |         30|                  10|         30|                  11|
|  9|[('consumer',), (2,)]            |         30|                  10|         30|                  17|
| 10|[('producer', 'transmitter'), (0, 1)]|         30|                  13|         30|                  13|
| 11|[('producer', 'transmitter'), (0, 2)]|         50|                  18|         50|                  18|
| 12|[('producer', 'transmitter'), (1, 0)]|         20|                  14|         15|                  14|
| 13|[('producer', 'transmitter'), (1, 2)]|         50|                  20|         50|                  20|
| 14|[('producer', 'transmitter'), (2, 0)]|         10|                  10|         15|                  13|
| 15|[('producer', 'transmitter'), (2, 1)]|         30|                  16|         30|                  16|
| 16|[('producer', 'consumer'), (0, 1)]|         30|                  15|         30|                  15|
| 17|[('producer', 'consumer'), (0, 2)]|         50|                  18|         50|                  18|
| 18|[('producer', 'consumer'), (1, 0)]|         80|                  10|         80|                  11|
| 19|[('producer', 'consumer'), (1, 2)]|         50|                  19|         50|                  19|
| 20|[('producer', 'consumer'), (2, 0)]|         20|                  10|         15|                  10|
| 21|[('producer', 'consumer'), (2, 1)]|         30|                  11|         30|                  11|
| 22|[('transmitter', 'consumer'), (0, 1)]|         60|                  18|         65|                  20|
| 23|[('transmitter', 'consumer'), (0, 2)]|         30|                   8|         30|                  10|
| 24|[('transmitter', 'consumer'), (1, 0)]|         40|                  17|         50|                  16|
| 25|[('transmitter', 'consumer'), (1, 2)]|         60|                  19|         65|                  16|
| 26|[('transmitter', 'consumer'), (2, 0)]|         60|                  16|         50|                  17|
| 27|[('transmitter', 'consumer'), (2, 1)]|         60|                  20|         65|                  20|
| 28|[('producer', 'transmitter', 'consumer'), (0, 1, 2)]|         50|                  17|         50|                  20|
| 29|[('producer', 'transmitter', 'consumer'), (0, 2, 1)]|         30|                  15|         30|                  15|
| 30|[('producer', 'transmitter', 'consumer'), (1, 0, 2)]|         50|                  20|         50|                  20|
| 31|[('producer', 'transmitter', 'consumer'), (1, 2, 0)]|         80|                  12|         80|                  14|
| 32|[('producer', 'transmitter', 'consumer'), (2, 0, 1)]|         30|                  13|         30|                  13|
| 33|[('producer', 'transmitter', 'consumer'), (2, 1, 0)]|         80|                   8|         80|                  10|

#### Recieved Information Winner

This bases the voting on the information that the consumers have recieved. The consumers can study this information to as what the world around them inclines to. 

In this methods, the consumers aggregate all the information that they have recieved thus far, and predict a winner based on it. This is interesting to study, as it presents what the transmitters are sending to the consumers.

Some conclusions from the data:
1. Recieved info winner aligns very closely with the actual winner, except in cases 33 and 31. This could be because of the fatal seperation that is cause by having high consumer bias with low transmitter bias and vice versa.
2. Otherwise, the data presents that the cummalitve nature of the information that the consumers are recieving is an accurate tool in reporting who will win, this is despite the network not adapting itself to suit the needs of the consumer.

|Num|                        Name                        |Recieved Info Winner|Num votes for Winner|Actual Winner|Num votes for Winner|
|--:|----------------------------------------------------|-------------------:|-------------------:|------------:|-------------------:|
|  0|None                                                |                  70|                  10|           60|                  11|
|  1|[('producer',), (0,)]            |                  10|                   9|           20|                   8|
|  2|[('producer',), (1,)]            |                  30|                  20|           30|                  20|
|  3|[('producer',), (2,)]            |                  50|                  20|           50|                  20|
|  4|[('transmitter',), (0,)]      |                  40|                  20|           60|                  16|
|  5|[('transmitter',), (1,)]      |                  70|                  12|           60|                  18|
|  6|[('transmitter',), (2,)]      |                  60|                  12|           50|                  16|
|  7|[('consumer',), (0,)]            |                  70|                  17|           70|                  19|
|  8|[('consumer',), (1,)]            |                  60|                  20|           30|                  10|
|  9|[('consumer',), (2,)]            |                  40|                  14|           30|                  10|
| 10|[('producer', 'transmitter'), (0, 1)]|                  30|                  20|           30|                  13|
| 11|[('producer', 'transmitter'), (0, 2)]|                  50|                  20|           50|                  18|
| 12|[('producer', 'transmitter'), (1, 0)]|                  20|                  20|           20|                  14|
| 13|[('producer', 'transmitter'), (1, 2)]|                  50|                  20|           50|                  20|
| 14|[('producer', 'transmitter'), (2, 0)]|                  10|                  14|           10|                  10|
| 15|[('producer', 'transmitter'), (2, 1)]|                  30|                  20|           30|                  16|
| 16|[('producer', 'consumer'), (0, 1)]|                  30|                  20|           30|                  15|
| 17|[('producer', 'consumer'), (0, 2)]|                  50|                  20|           50|                  18|
| 18|[('producer', 'consumer'), (1, 0)]|                  20|                  19|           80|                  10|
| 19|[('producer', 'consumer'), (1, 2)]|                  50|                  20|           50|                  19|
| 20|[('producer', 'consumer'), (2, 0)]|                  20|                  14|           20|                  10|
| 21|[('producer', 'consumer'), (2, 1)]|                  30|                  20|           30|                  11|
| 22|[('transmitter', 'consumer'), (0, 1)]|                  60|                   9|           60|                  18|
| 23|[('transmitter', 'consumer'), (0, 2)]|                  30|                  13|           30|                   8|
| 24|[('transmitter', 'consumer'), (1, 0)]|                  30|                  17|           40|                  17|
| 25|[('transmitter', 'consumer'), (1, 2)]|                  60|                  17|           60|                  19|
| 26|[('transmitter', 'consumer'), (2, 0)]|                  60|                  17|           60|                  16|
| 27|[('transmitter', 'consumer'), (2, 1)]|                  60|                  19|           60|                  20|
| 28|[('producer', 'transmitter', 'consumer'), (0, 1, 2)]|                  50|                  20|           50|                  17|
| 29|[('producer', 'transmitter', 'consumer'), (0, 2, 1)]|                  30|                  20|           30|                  15|
| 30|[('producer', 'transmitter', 'consumer'), (1, 0, 2)]|                  50|                  20|           50|                  20|
| 31|[('producer', 'transmitter', 'consumer'), (1, 2, 0)]|                  10|                  13|           80|                  12|
| 32|[('producer', 'transmitter', 'consumer'), (2, 0, 1)]|                  30|                  20|           30|                  13|
| 33|[('producer', 'transmitter', 'consumer'), (2, 1, 0)]|                  10|                  16|           80|                   8|

#### Ask Random Consumers
This method asked the consumers to consult other random consumers and ask them who they think will win. 

Conclusions:
1. This stratergy was mostly effective in guessing who the winner will be. 
2. But it did not present any new/unique datapoints.


|Num|                        Name                        |Neibhours Winner|Real Winner|Num votes for Winner|
|--:|----------------------------------------------------|---------------:|----------:|-------------------:|
|  0|None                                                |              50|         60|                  11|
|  1|[('producer',), (0,)]            |              65|         20|                   8|
|  2|[('producer',), (1,)]            |              30|         30|                  20|
|  3|[('producer',), (2,)]            |              50|         50|                  20|
|  4|[('transmitter',), (0,)]      |              50|         60|                  16|
|  5|[('transmitter',), (1,)]      |              50|         60|                  18|
|  6|[('transmitter',), (2,)]      |              50|         50|                  16|
|  7|[('consumer',), (0,)]            |              80|         70|                  19|
|  8|[('consumer',), (1,)]            |              30|         30|                  10|
|  9|[('consumer',), (2,)]            |              30|         30|                  10|
| 10|[('producer', 'transmitter'), (0, 1)]|              80|         30|                  13|
| 11|[('producer', 'transmitter'), (0, 2)]|              50|         50|                  18|
| 12|[('producer', 'transmitter'), (1, 0)]|              15|         20|                  14|
| 13|[('producer', 'transmitter'), (1, 2)]|              50|         50|                  20|
| 14|[('producer', 'transmitter'), (2, 0)]|              15|         10|                  10|
| 15|[('producer', 'transmitter'), (2, 1)]|              30|         30|                  16|
| 16|[('producer', 'consumer'), (0, 1)]|              30|         30|                  15|
| 17|[('producer', 'consumer'), (0, 2)]|              50|         50|                  18|
| 18|[('producer', 'consumer'), (1, 0)]|              80|         80|                  10|
| 19|[('producer', 'consumer'), (1, 2)]|              50|         50|                  19|
| 20|[('producer', 'consumer'), (2, 0)]|              15|         20|                  10|
| 21|[('producer', 'consumer'), (2, 1)]|              80|         30|                  11|
| 22|[('transmitter', 'consumer'), (0, 1)]|              65|         60|                  18|
| 23|[('transmitter', 'consumer'), (0, 2)]|              30|         30|                   8|
| 24|[('transmitter', 'consumer'), (1, 0)]|              50|         40|                  17|
| 25|[('transmitter', 'consumer'), (1, 2)]|              65|         60|                  19|
| 26|[('transmitter', 'consumer'), (2, 0)]|              50|         60|                  16|
| 27|[('transmitter', 'consumer'), (2, 1)]|              65|         60|                  20|
| 28|[('producer', 'transmitter', 'consumer'), (0, 1, 2)]|              50|         50|                  17|
| 29|[('producer', 'transmitter', 'consumer'), (0, 2, 1)]|              30|         30|                  15|
| 30|[('producer', 'transmitter', 'consumer'), (1, 0, 2)]|              50|         50|                  20|
| 31|[('producer', 'transmitter', 'consumer'), (1, 2, 0)]|              80|         80|                  12|
| 32|[('producer', 'transmitter', 'consumer'), (2, 0, 1)]|              30|         30|                  13|
| 33|[('producer', 'transmitter', 'consumer'), (2, 1, 0)]|              65|         80|                   8|

## Conclusion
As we can see form the methods used to analyze the structure of these above, the producers and transmitters have a huge affect on the final inclinations of a population. This is espescially true when the transmitters are biased, as they cause the most drastic affect in the final inclinations of the consumers. They are also in turn affected by Producers, but as they are few in number it is not as apparent. Further work would be in varying the numbers of Producers, Transmitters and Consumers, this would be trivial to implement with only changing the initial number passed in. Other work would require making the transmitters adaptive, so they try to find the balance between luring consumers far away from them to their 'preferred' inclination and maintaining the current crop of consumers that they already have. Other work could also consider that each consumer might have a preferred transmitter, so tranmitters would have to compete for consumers.

## References
1. DellaVigna, Stefano, and Ethan Kaplan. “The Fox News Effect: Media Bias and Voting.” _The Quarterly Journal of Economics_, vol. 122, no. 3, 2007, pp. 1187–1234. _JSTOR_, JSTOR, www.jstor.org/stable/25098871.
2. CHIANG, CHUN-FANG, and BRIAN KNIGHT. “Media Bias and Influence: Evidence from Newspaper Endorsements.” _The Review of Economic Studies_, vol. 78, no. 3, 2011, pp. 795–820. _JSTOR_, JSTOR, www.jstor.org/stable/23015831.
3. Wilner, Tamar. “We Can Probably Measure Media Bias. But Do We Want to?” _Columbia Journalism Review_, 9 Jan. 2018, www.cjr.org/innovations/measure-media-bias-partisan.php?page=all.

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTUxMDk0NzcsLTkwODQ3MTE4NCwtMTExNj
A2NzY0LC0xNDk0NTU5MTE4LC0xMTAyODgwOTYsLTIwOTgwMTE5
NTQsMTgwNzIwMjU4NV19
-->