---
created: 2024-08-03T10:10:11 (UTC -03:00)
tags: [ai,python,meta,nlp,software,coding,development,engineering,inclusive,community]
source: https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest
author: 
---

# How to Run Llama-3.1🦙 Locally Using Python🐍 and Hugging Face 🤗 - DEV Community

> ## Excerpt
> Introduction   The latest Llama🦙 (Large Language Model Meta AI) 3.1 is a powerful AI model...

---
[![Cover image for How to Run Llama-3.1🦙 Locally Using Python🐍 and Hugging Face 🤗](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftd8ijurnjzzcn6lkdp96.jpeg)](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftd8ijurnjzzcn6lkdp96.jpeg)

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#introduction)Introduction

The latest Llama🦙 (Large Language Model Meta AI) 3.1 is a powerful AI model developed by Meta AI that has gained significant attention in the natural language processing (NLP) community. It is the most capable open-source llm till date. In this blog, I will guide you through the process of cloning the Llama 3.1 model from Hugging Face🤗 and running it on your local machine using Python. After which you can integrate it in any AI project.

___

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#prerequisites)Prerequisites

-   Python 3.8 or higher installed on your local machine
-   Hugging Face Transformers library installed (`pip install transformers`)
-   Git installed on your local machine
-   A Hugging Face account

___

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#step-1-get-access-to-the-model)Step 1: Get access to the model

-   Click here [https://huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct) to open the official hugging face repository of Meta's Llama-3.1-8B-Instruct (you can use other llama 3.1 models in the same way).

[![Meta-llama-3.1-8b-Instruct hugging face model](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fo0lpys2y71s4cte30ex7.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fo0lpys2y71s4cte30ex7.png)

-   At the beginning you should be seeing this:

[![Meta-llama-3.1-8b-Instruct model](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgky1nxjvf28r46cuiwtp.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgky1nxjvf28r46cuiwtp.png)

-   Submit the form below to get access of the model

[![access to meta llama 3.1 model](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F00byphoel5c8s0nfa7xw.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F00byphoel5c8s0nfa7xw.png)

-   Once you see **"You have been granted access to this model"**, you are good to go...

[![gated model in hugging face](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnsttr9arstf3s7r2lp2t.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnsttr9arstf3s7r2lp2t.png)

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#step-2-create-an-accesstoken)Step 2: Create an ACCESS\_TOKEN

-   Go to **"Settings"** (Bottom right corner of the below image):

[![hugging face settings](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8mfn10g0l952jn7drses.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8mfn10g0l952jn7drses.png)

-   Go to **"Access Tokens"** click **"Create new token"**(upper right corner of the image):

[![create hugging face token](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fo43nhk25cg8e57i6u1r3.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fo43nhk25cg8e57i6u1r3.png)

-   Give read and write permissions and **select the repo** as shown:

[![create hugging face token](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg15k9gxmp8vi8gdbx3cw.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg15k9gxmp8vi8gdbx3cw.png)

-   Copy the token and place it somewhere safe and secure as it will be needed in the future.(**note:** once you copy it you cannot copy it again, so if you anyhow forget the key, you have to create a new one to begin with :))

[![huggingface token](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F175lizyojzlbkzy95pxw.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F175lizyojzlbkzy95pxw.png)

___

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#step-3-clone-the-llama-31-model)Step 3: Clone the LLaMA 3.1 Model

Now run the following command on your favorite terminal.  
The `ACCESS_TOKEN` is the one you copied and the `<huggingface-user-name>` is the username of your hugging face account.  

```
git clone https://&lt;huggingface-user-name&gt;:&lt;ACCESS_TOKEN&gt;@huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct
```

This can take a lot of time depending on your internet speed.

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#step-4-install-required-libraries)Step 4: Install Required Libraries

Once the cloning is done, go to the cloned folder and install all the dependencies from the `requirements.txt`. (you can create an virtual-environment using conda(**recommended**) or virtualenv)

Using **conda**:  

```
<span>cd </span>Meta-Llama-3.1-8B-Instruct
conda <span>install</span> <span>--yes</span> <span>--file</span> requirements.txt
```

Using **pip**:  

```
<span>cd </span>Meta-Llama-3.1-8B-Instruct
pip <span>install</span> <span>-r</span> requirements.txt
```

___

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#step-5-run-the-llama-31-model)Step 5: Run the Llama 3.1 Model

Create a new Python file (e.g., test.py) and paste the location of the model repository you just cloned as the `model_id` (such as, `"D:\\Codes\\NLP\\Meta-Llama-3.1-8B-Instruct"`). Here is an example:  

```
<span>import</span> <span>transformers</span>
<span>import</span> <span>torch</span>

<span>## Here you paste your cloned repos location
</span><span>model_id</span> <span>=</span> <span>"</span><span>D:</span><span>\\</span><span>Codes</span><span>\\</span><span>NLP</span><span>\\</span><span>Meta-Llama-3.1-8B-Instruct</span><span>"</span> 

<span>pipeline</span> <span>=</span> <span>transformers</span><span>.</span><span>pipeline</span><span>(</span>
    <span>"</span><span>text-generation</span><span>"</span><span>,</span>
    <span>model</span><span>=</span><span>model_id</span><span>,</span>
    <span>model_kwargs</span><span>=</span><span>{</span><span>"</span><span>torch_dtype</span><span>"</span><span>:</span> <span>torch</span><span>.</span><span>bfloat16</span><span>},</span>
    <span>device_map</span><span>=</span><span>"</span><span>auto</span><span>"</span><span>,</span>
<span>)</span>

<span>messages</span> <span>=</span> <span>[</span>
    <span>{</span><span>"</span><span>role</span><span>"</span><span>:</span> <span>"</span><span>user</span><span>"</span><span>,</span> <span>"</span><span>content</span><span>"</span><span>:</span> <span>"</span><span>Who are you?</span><span>"</span><span>},</span>
<span>]</span>

<span>outputs</span> <span>=</span> <span>pipeline</span><span>(</span>
    <span>messages</span><span>,</span>
    <span>max_new_tokens</span><span>=</span><span>256</span><span>,</span>
<span>)</span>
<span>print</span><span>(</span><span>outputs</span><span>[</span><span>0</span><span>][</span><span>"</span><span>generated_text</span><span>"</span><span>][</span><span>-</span><span>1</span><span>])</span>
```

You can set `device_map=cuda` if you want use the gpu also.

Step 6: Run the Python Script  

### [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#output)Output

[![llama-3.1 output](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fs2145xgibavplg0bkjvp.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fs2145xgibavplg0bkjvp.png)

___

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#issues-you-can-face)Issues you can face

-   `OSError: [WinError 126] fbgemm.dll`
    -   To solve this error make sure you have **Visual Studio** installed.
        -   In case you don't have it, [click here](https://visualstudio.microsoft.com/vs/community/) and install it.
        -   Then restart the computer.
-   If there is still any errors with pytorch versions, use [anaconda](https://www.anaconda.com/) or [miniconda](https://docs.anaconda.com/miniconda/miniconda-install/) to configure a new environment with suitable python version and dependencies.
-   If you are facing any other issue or error feel free to comment below.

___

> ## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#resources)Resources
> 
> For more details on llama 3.1 check out: [https://ai.meta.com/blog/meta-llama-3-1/](https://ai.meta.com/blog/meta-llama-3-1/)

___

## [](https://dev.to/debapriyadas/cloning-and-running-llama-31-model-from-hugging-face-using-python-3m80?context=digest#conclusion)Conclusion

In this blog, we have successfully cloned the LLaMA-3.1-8B-Instruct model from Hugging Face and run it on our local machine using Python. You can now experiment with the model by modifying the prompt, adjusting hyperparameters, or integrate with your upcoming projects. Happy coding!
