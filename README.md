# LLM-saliency-map

This project implements a simple version of saliency maps for GPT2

This is implemented in the following steps:
1. Get text in one of the following ways
    1. Write your own text
    2. Get the model to generate text for you, either by using the generate_response or interactive_session() methods
2. Select a token/tokens to be analyzed
3. Analyze and visualize using the visualize_saliency() method
    1. The method calls calculates the derivatives of the tokens in the text with respect to the selected token(s)
    2. The magnitudes of the derivatives are then calculated and normalized
    3. These magnitudes are then vizualised using a heatmap


### Example output
![image](https://github.com/user-attachments/assets/883f2089-69d2-4edd-bc92-74145a977d6f)
The prompt was "The United States of America is the", then the tokens marked in blue was chosen to be analyzed.

## Why could this be useful

A saliency map is a useful interpretability tool since it allows for easy visualization of what parts of the input made the model make the decision it made. Saliency maps are traditionally used in vision models however, I believe they could be very useful for LLMs as well. One usecase could for example be to get a better understanding of why a model made a certain judgement. Let's for example say an AI judge deems a man guilty based on some text-based evidence. By creating a saliency map where the token to analyze is the judgement then the saliency map should indicate what the model deems to be the most important pieces of evidence to support the judgement. 

Another usecase can be for safety-testing, let's say we have the same setup as previously where there's an AI judge who makes a judgement based on some text-based evidence. By, again, creating a saliency map where the token to analyze is the judgement we can see if the judgement is affected by factors which shouldn't impect the decision like for example ethnicity. Using a saliency map should be a very relatively easy way to do this safety-testing compared to an alternative like changing all these factors on the evidence supplied to the models and then re-running the model and checking if the output is changing. The second approach seems to be much more costly since one actively has to identify different factors to change, then change them and rerun the models for each of these changes, preferably multiple times to ensure an acceptable level of statistical significance.

The third and last usecase, which is most relevant to this project is for curiosity and learning. Because of the huge complexity of these models any tool which helps to understand how they work is (at least to me) interesting and valuable. The project has lead to a few insights like:
1. The first token in the sequence does often seem to be quite important, my hypothesis is that this is because it's the highest entropy token (because it's not conditioned on anything) and it sets the language, topic and tone.
2. When analyzing a single token at a time the saliency maps aren't as clear and distinctive as I thought. I generally expected most tokens to have an importance of very close to 0 and then just one or a very few to have higher values however, usually quite a significant portion of tokens seem to have an importance of at least 0.25 even with (according to me) longer dependencies. To me this shows the importance of semantichs and how complex langauge with tons of both short and long dependencies. When adding more tokens to analyze the heatmap looks more like I'd expected.


## Limitations and improvements
1. One only obtains the saliency map for the tokens before the analyzed token. This makes sense because of the autoregressive structure of langage but this is a big difference compared to saliency maps in vision models where one gets the saliency for the whole image.
2. UI should be improved, clicking on the individual tokens works pretty good but when many tokens are to be selected this becomes tidious. Also the buttons aren't as easy to read as regular text, the readability of the buttons should be improved
