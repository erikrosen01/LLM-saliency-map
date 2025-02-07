# LLM-saliency-map

This project implements a simple version of saliency maps for GPT2

This is implemented in the following steps:
1. Get text in one of the following ways
    1. Write your own text
    2. Get the model to generate text for you, either by using the generate_response or interactive_session() methods
2. Select a token to be analyzed
3. Analyze and visualize using the visualize_saliency() method
    1. The method calls calculates the derivatives of the tokens in the text with respect to the selected token
    2. The magnitudes of the derivatives are then calculated and normalized
    3. These magnitudes are then vizualised using a heatmap


## Why could this be useful

A saliency map is a useful interpretability tool since it allows for easy visualization of what parts of the input made the model make the decision it made. Saliency maps are traditionally used in vision models however, I believe they could be very useful for LLMs as well. One usecase could for example be to get a better understanding of why a model made a certain judgement. Let's for example say an AI judge deems a man guilty based on some text-based evidence. By creating a saliency map where the token to analyze is the judgement then the saliency map should indicate what the model deems to be the most important pieces of evidence to support the judgement. 

Another usecase can be for safety-testing, let's say we have the same setup as previously where there's an AI judge who makes a judgement based on some text-based evidence. By, again, creating a saliency map where the token to analyze is the judgement we can see if the judgement is affected by factors which shouldn't impect the decision like for example ethnicity. Using a saliency map should be a very relatively easy way to do this safety-testing compared to an alternative like changing all these factors on the evidence supplied to the models and then re-running the model and checking if the output is changing. The second approach seems to be much more costly since one actively has to identify different factors to change, then change them and rerun the models for each of these changes, preferably multiple times to ensure an acceptable level of statistical significance.

The third and last usecase, which is most relevant to this project is for curiosity and learning. Because of the huge complexity of these models any tool which helps to understand how they work is (at least to me) interesting and valuable. The project has lead to a few insights like:
1. The first token in the sequence does often seem to be quite important, my hypothesis is that this is because it's the highest entropy token (because it's not conditioned on anything) and it really sets the direction regarding text should come next.
2. The saliency maps aren't as clear and distinctive as I thought. I generally expected most tokens to have an importance of very close to 0 and then just one or a very few to have higher values however, usually quite a significant portion of tokens seem to have an importance of at least 0.25 even with (according to me) longer dependencies. To me this shows how complex langauge with tons of both short and long dependencies.


## Limitations and improvements
1. One only obtains the saliency map for the tokens before the analyzed token. This makes sense because of the autoregressive structure of langage but this is a big difference compared to saliency maps in vision models where one gets the saliency for the whole image.
2. Currently one can only get saliency of one token at a time. This means one can't get the saliency for a judgement/statement which spans multiple tokens, this could be implemented by allowing the user to select multiple tokens, then calculate the saliencies for each token individually and, summing the saliency and then visualizing this summation. Also one can only get the saliency for the first occurance of each token.
3. UI is pretty bad, currently one has to type in the token which is pretty hard given that the tokenization is not given. It would be interesting to have a more interactable page in which the user can highlight any text (including multiple tokens) and then the saliency map is displayed by changing the color of the text and not by generating an image like in this case.
