===================
Activation Sparsity
===================

Definition
----------
During execution, the average sparsity of neuron activations over all neurons in all model layers, for all timesteps of all tested samples, where 0 refers to no sparsity (i.e., all neurons are always activated), and 1 refers to the case where all neurons have a zero output.

The sparsity is calculated by accumulating the number of zero activations (:math:`z`), over all neuron layers (:math:`l`), timesteps (:math:`t`), and input samples (:math:`i`) and dividing by the total number of neurons (:math:`N`), :math:`\frac{\sum_l \sum_t \sum_i z_{l,t}^i}{\sum_t \sum_l \sum_i N_{l,t}^i}`.

Implementation Notes
--------------------
When the NeuroBench model is instatiated, certain layers can be recognized automatically for activation sparsity calculation.

These layers are:
    - nn.ReLU
    - nn.Sigmoid
    - snn.SpikingNeuron (this is the parent class for all spiking neuron models)

Activation sparsity is calculated using nn.Modules and not torch.functional. 

To add custom activation modules, use `NeuroBenchModel.add_activation_module()` on your wrapped model.

The activation sparsity is calculated by using hooks that save the outputs of the activation modules. 
At the end of the workload, we total the number of activations that are zero by summing over all modules, over all batches, and divide by the total number of activations summed over all modules, over all batches. 
