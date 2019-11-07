# Graphical-models
Here we will learn some of the graphical networks along with there usage
## Bayesian Networks
### CREATING CPD 

from pgmpy.factors.discrete import TabularCPD
performane_cpd = TabularCPD( variable = 'Performance',
                           variable_card = 3,
                           values = [[.5, .8, .8, .9],
                                    [.3, .15, .1, .08],
                                    [.2, .05, .1, .02]],
                           evidence = ['Genetic', 'Practice'],
                           evidence_card = [2,2])
print(performance_cpd)
![git1](https://user-images.githubusercontent.com/52886443/68379628-3f6a8100-0174-11ea-8b9a-3fc80670c262.png)

### Creating a Bayesian model
from pgmpy.models import BayesianModel
model = BayesianModel()

--> to add a node to your network
model.add_nodes_from(['performance', 'practice', 'genetics', 'offer'])
model.add_edges_from([('Genetics', 'Performance'), ('Performance', 'offer'), ('Practice', 'Performance')])

### Watching how your network looks

import networkx as nx
nx.draw_networkx(model)
![git2](https://user-images.githubusercontent.com/52886443/68380618-32e72800-0176-11ea-939d-8d3632618d00.png)

### Adding cpds to your model

model.add_cpds(genetic_cpd, performance_cpd)
model.check_model() //to check if your model has all nodes with edges or not

model.get_cpds('performance') //specifying node to see the cpds of particular node

model.get_independencies()  // to get the independencies off all elements

model.local_independencies('performance')  //to see the independency of a particular element
![git3](https://user-images.githubusercontent.com/52886443/68381048-f831bf80-0176-11ea-9a57-8a8dcd13d5f7.png)

## Making Inferences

Inference is drawing conclusion from your models
Knowing some preconditions we can predict the other

from pgmpy.inference import VariableElimination
infer = VariableInference(model)
infer_offer = infer.query(variable = ['infer'])
print(infer_offer['offer'])

![git4](https://user-images.githubusercontent.com/52886443/68381892-9ffbbd00-0178-11ea-9cc4-0b592e5309f4.png)

-->Using evidence as well
offer_genetics_false = infer.query(variable = ['infer'], evdence = {'Genetics':1})
print(offer_genetics_false['infer'])

![git5](https://user-images.githubusercontent.com/52886443/68382154-257f6d00-0179-11ea-9970-970408307945.png)











