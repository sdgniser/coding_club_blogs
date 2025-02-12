---
layout: post
title:  "Learning about APIs using Ollama"
date:   2025-02-12 07:00:00 +0530  # not taking risk
categories: blogs
author: 'Aritra Mukhopadhyay'
abstract: "Introduction to APIs through the Ollama API and besides learning about the basics of API usage, also learning about how to include LLM output in any program."
---


<!-- ![Cover Picture]({{site.baseurl}}/assets/img/lrandgd/ml01.png) -->
<!-- ![Cover Picture](../assets/img/api_ollama/2024-12-10_21-28-35_2124.png) -->


In today's digital landscape, APIs have become the backbone of modern software development. Meanwhile, Large Language Models (LLMs) have revolutionized natural language processing. LLMs like OpenAI's ChatGPT, Anthropic's Claude, and Google's Gemini are changing the way we interact with technology. In this blog post, we'll explore APIs through the example of Ollama API, which allows us to generate LLM completions *locally*. This gives us privacy and more control over our data. By using Ollama, we can unlock the power of LLMs right on our own machines, without relying on external servers. Here is a brief outline of what we will cover in this blog post:

- [What is an API?](#what-is-an-api)
- [Ollama API](#ollama-api)
  - [Installing Ollama](#installing-ollama)
  - [Running Ollama Server](#running-ollama-server)
  - [Downloading some models](#downloading-some-models)
- [Programmatically prompting the Ollama API](#programmatically-prompting-the-ollama-api)
- [Conclusion](#conclusion)


## What is an API?

An **Application Programming Interface**, or API, is a set of defined rules and protocols that enables different software or systems to communicate with each other. In essence, an API allows one system to request services or data from another system, without having to know the intricacies of how that service is provided. To illustrate this concept, consider a real-world example: when you visit the NISER canteen to order a cup of tea, the canteen staff don't have to worry about producing their own milk and tea leaves from scratch. Instead, they outsource these tasks to specialized suppliers, allowing them to focus on preparing your tea quickly and efficiently. Similarly, in the world of software development, APIs enable us to outsource specific tasks or services to other systems, freeing us up to concentrate on our core project.

For instance, when building a drone here at RTC, we used a Global Positioning System (GPS) API to enable automatic stabilized flight modes, without having to develop our own GPS technology from scratch. Similarly, companies like Zomato and Uber rely on APIs for navigation and weather forecasting. Both use GPS to guide its delivery personnel, while Uber also uses weather APIs to adjust its pricing in real-time, increasing fares during rainy days.

By leveraging these APIs, developers can focus on building their core product, rather than getting distracted in the details of implementing every single detail from scratch. This concept extends even to libraries and frameworks, which can be thought of as internal APIs - for example, when working with matrices in Python, we might use NumPy's built-in functions to calculate eigenvalues and eigenvectors, without having to delve into the complexities of eigenvalue decomposition ourselves. By using these APIs and libraries, we can streamline our development process, reduce errors, and build more robust and efficient software systems.

There are different types of API services which provide you with some data or get some calculation done either from a remote server or locally, like weather API, location API, etc. In this blog post, we'll focus on the Ollama API, which allows us to generate LLM completions locally either on our own machines or on another computer on the same network.

## Ollama API

In general, the entire OLLAMA API Documentation is available [here](https://github.com/ollama/ollama/blob/main/docs/api.md), but today we will focus on the **[generate API](https://github.com/ollama/ollama/blob/main/docs/api.md#generate-a-completion)**. To explore all other functions, feel free to visit the documentation page. The goal here is simple: we will write a function that takes in a prompt and returns the completion. This function will use the Ollama API to generate the text, but the user won't need to know how it's done — they will simply provide a prompt and get the answer back, making the process seamless and easy.

### Installing Ollama

For Linux, Mac and Windows users, you can download the ollama softwares from the official download page following the instructions as mentioned here: [ollama download](https://ollama.com/download/) and install it on your machine.

I prefer a different way of installing Ollama on Linux instead of following the official method. I like to keep the binaries and downloaded models in a separate folder within my home directory. Here's why:  

- **Easy removal & cleanup** - Keeping everything in a single folder allows for quick and complete removal by simply deleting that folder.  
- **More control over the server** - Since it's not added as a system service, I have full control over when to start or stop the server, avoiding unnecessary background processes.  
- **No need for sudo access** - This method works entirely within the user space, making it ideal for systems where administrative privileges are restricted.  

So, that's why I prefer this approach. Here's how I do it:  

```bash
mkdir ~/.ollama  # make a directory in the home directory
cd ~/.ollama  # go to the directory
# download the ollama binaries (1.5GB file might take some time):
curl -L https://ollama.com/download/ollama-linux-amd64.tgz -o ~/.ollama/ollama_download.tgz
tar -xvzf ollama_download.tgz  # extract the downloaded file
mv bin/ollama ollama  # move the binary to ~/.ollama
rm -rf bin lib  # remove the bin and lib directories
```

add the following line to your `.bashrc` if you are using bash or the respective shell startup file for your shell to add the `ollama` binary to your path:

```bash
export PATH="$PATH:$HOME/.ollama"
```

### Running Ollama Server

Here onwards all the commands will be specific to linux only, but most of them have good probability to work as well on Mac and Windows (although not tested).

To start the Ollama server, run the following command:

```bash
ollama serve
```

this will open a server at [`http://127.0.0.1:11434`](http://localhost:11434) by default. This is only accessible from your local machine. However if you want to run the ollama server in a remote PC which has big GPUs and access it from your local machine or multiple small machines on the same network, while the heavy lifting is being done by the big machine, you can run the following command:

```bash
OLLAMA_HOST="x.x.x.x:port" ollama serve
```

where `x.x.x.x` is the public IP address of the server PC. This will open the server at `http://x.x.x.x:port` and you can access it from your local machine or any other machine on the same network.

if you visit this website using a browser, you will see the response `Ollama in running!` which means the server is up and running.

**You need to keep the terminal where you had ran `ollama serve` running always for the following steps to work.** For this either you can keep it open manually or you might want to use `tmux`. To know more refer to [this](https://sdgniser.github.io/coding_club_blogs/blogs/2024/01/06/ssh-servers.html#tmux) part of Sagar's talk on `Remote Devlopment on SSH Servers`.

### Downloading some models

Now that we have the ollama server running we need to get some models to generate completions. You can always visit the [ollama model library](https://ollama.com/search) to download models you like. Here are some guidelines to find out which models will be perfect for you:

- Larger models have higher number of parameters, so they can generate more accurate completions, but they are slower, larger download and storage size and need more memory to run.
- Smaller models although inaccurate, are faster and need less memory to run.
- If prompted correctly, smaller models can also generate quite accurate completions, but might not be very knowledgeable or be able to do complicated integrations.
- If a model has 5GB of download size, it will need exactly that much amount of memory to run. So if you have a GPU with more than that amount of VRAM, you can run that model on your GPU, otherwise you can run it on CPU if you have that much amount of RAM (CPU is generally not very efficient in running these things).

In this tutorial let us stick to the `llama3.1:8B` model which is a 8 billion parameter model. To download this model, run the following command:

```bash
ollama pull llama3.1:8B
```

This will download the model and store it in the `~/.ollama/models` directory. You can always check the models you have downloaded by running the following command:

```bash
ollama list
```

You can also directly run the models in the terminal by running the following command:

```bash
ollama run llama3.1:8B
```

This will download the model if not already downloaded and run the model in the terminal. You can type in the prompt and get the completion in the terminal itself.

## Programmatically prompting the Ollama API

Now that we have the server running and the model downloaded, we can now write scripts to prompt the Ollama API. We will see examples in python how to 

```python
import requests
import json

class Ollama:
    def __init__(self, api_url="http://localhost:11434/", model_name="llama3.1"):
        self.api_url = api_url if api_url.endswith("/") else f"{api_url}/"
        self.model_name = model_name

    def generate(self, prompt, system=None, temperature=0.2):
        system = system or "You are a very helpful assistant."
        url = f"{self.api_url}api/generate"
        headers = {"Content-Type": "application/json"}
        data = {
            "model": self.model_name,
            "prompt": prompt,
            "system": system,
            "temperature": temperature,
            "stream": False  # Always set stream to False
        }

        response = requests.post(url, headers=headers, data=json.dumps(data))

        if response.status_code == 200:
            return response.json()["response"]
        else:
            raise Exception(f"Failed to generate text: {response.text}")

ollama = Ollama()  # Initialize with default URL and model name

completion = ollama.generate(
    prompt = "What colour is the sky?",
    system = "Keep you responses very short and to the point."
)  # Ay mi amor! ay mi amor!
print(completion)
```

## Conclusion

Here I have shown a simple example of how to prompt the Ollama API using Python and JavaScript. This blog was about APIs and how to use them. We demonstrated this with the Ollama API, but there are countless APIs available for different use cases. Ultimately, using an API comes down to reading and understanding its documentation, then integrating it into your code. I hope this blog was helpful in understanding the basics of APIs and how to use them effectively.

However, we have only covered the **Generate API** of Ollama. You can expand the class to include all other available API endpoints for more advanced functionality. If your interest lies purely in using LLMs, there's an easier way—Ollama provides an official [Python library](https://pypi.org/project/ollama/) that simplifies interactions with the API.

On the other hand, if you are looking to completely replace services like ChatGPT with local LLMs, you can use more powerful models like **llama3.3:70B**, which performs comparably to GPT-4. For a seamless chat experience, you can integrate it with UIs like [Open WebUI](https://github.com/open-webui/open-webui) or [Chatbox](https://chatboxai.app/en).

Happy coding! 🚀
