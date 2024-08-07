# SLMinAzureContainerApps

This is a quick, rough guide on running a SLM (Phi3) in Azure Container Apps with the model sitting in a file share. I was specifically intersted in just using CPU and seeing what the performance was like. Azure Container Apps does support GPU for better performance. But I was intrigued by CPU perf and of course running a SLM yourself and what potential use cases my arise from this. 

## Download Phi3 and put in Azure Files

I used the mini Phi3 which is quantized (4bits) in GGUF format, available here [Phi3 mini GGUF](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct-gguf) 

After downloading, I put the model in an Azure Files share. For this and the rest of setting this set-up, I generally followed this tutorial [Tutorial: Create an Azure Files volume mount in Azure Container Apps](https://learn.microsoft.com/en-us/azure/container-apps/storage-mounts-azure-files?tabs=bash)

## llama.cpp server container

I used the llama-server container built and published as part of llama.cpp. [LLaMA.cpp HTTP Server](https://github.com/ggerganov/llama.cpp/blob/master/examples/server/README.md) 

You will need to gain access to the GitHub package. (I had to use a version from a couple of weeks ago because of a sliding window issue that may be fixed now.) [llama.cpp server] (https://github.com/ggerganov/llama.cpp/pkgs/container/llama.cpp/244188617?tag=server--b1-37b12f9)

## Modify the Azure Container App 

I created a consumption Azure Container App according to the tutorial above, and then I just had to make sure that the command and args were set properly. This needs to be done via YAML. I used the Azure console built-in to the Portal. 

For this model, it seems 2 core and 4GB were the right amount. 

The llama.cpp server generally supports the OpenAI API (completion endpoint). I tested it out with curl but it seems like it can be used with some packages (autogen). The performance I got in this setup was reported by the server as ~7 tokens per second. 


