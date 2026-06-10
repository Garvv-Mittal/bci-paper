# EEG NET

OBJECTIVE-

This paper aimed to develop a single lightweight CNN model because what usually happens is that we need different ai models for different bci task . so the aim was to make a single CNN model that could handle each and every type of EEG task and can classify brain signals in correct category. 

The researchers also wanted to verify that EEGnet was accepting real and meaningful brain signal patterns, not noise or artifacts so that decision could be trusted and understood.

METHODOLOGY-

1. EEGnet is a type of CNN designed specifically for EEG brain signals.  it uses technology to extract important brain signals while keeping the model lightweight. perform better that usual deep leaning models.
2. researcher wanted to check whether  EEg net works well for different brain signals , so the test was conducted on 4 different bcs models.

P300(VISUAL EVOKED POTENTIALS)

this appears when a person notices an important visual stimulus.

ERN(ERROR RELATED NEGATIVITY)

this appears when a person realises that they made an error.

MRCP( MOVEMENT- RELATED CORTICAL POTENTIAL)

this signal appears before and during movement.

SMR(SENSORIMOTOR RHYTHM)

these are rhythmic brain signals related to movement or imagined movement.

using different datasets of different sizes to determine whether the model could perform well.

1. the researchers compared EEGnet with other existing methods-

Deep-convnet , ShallowConvnet ,xDrawn+riemann , fbcsp. which help them to compare.

1. feature explainability was used to analyse and understand the features learned by EEGnet , ensuring that the model decision were based on meaningful EEG patterns.

RESULTS 

1. the researchers found that EEGnet could work well on many different types of brain signals [tasks.it](http://tasks.it) was better than the original method and also effective when only a small amount of training data was available. it successfully handled both ERP and oscillatory based bcs applications.
2. The researchers found that EEGnet is both small and smart. it achieved performance similar to larger CNN models while using 100 times less parameters. they also showed that EEGnet accepts real and meaningful data not noise——> because EEGnet features closely resembles those identified by established FBCSP method, which clearly means model was capturing meaningful brain signals rather than noise.
3. it was strong in P300 and MRCP tasks, while traditional xDAWN+RG method superior for ERN classification.

THIS PAPER PROPOSES EEGNET ( A COMPACT CNN MODEL FOR EEG BASED BRAIN COMPUTER INTERFACE) IT CAN WORK ON DIFFERENT BCI TASK AND GIVE GOOD ACCURACY EVEN WHEN A SMALL AMOUNT OF DATA IS AVAILABLE.

USES FEWER PARAMETERS THAN THE USUAL METHOD — MAKING IT FASTER AND MOR E EFFICIENT.