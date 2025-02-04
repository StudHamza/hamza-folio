---
layout: page
title: Los valienties
description: LLM-powered digital assistant with Amazon Bedrock to build teams and answer questions about VALORANT Esports players
img: assets/img/VCT_Hack/logo.png
importance: 1
category: software
---

### Hackathon Project Summary: Data Analysis and AWS Architecture
This project is a sumbmission of the VCT 2024 hackathon, we where required to build a LLM-powered digital assistant with Amazon Bedrock to build teams and answer questions about VALORANT Esports players.
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/VCT_Hack/chatapp.png" title="Los Valienties" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Final Streamlit app
</div>

#### Initial Planning and Data Analysis
In the beginning, the hackathon project focused on understanding the vast amount of data available (***101 GB compressed**). Using the `jq` command, patterns and features were extracted to build a knowledge base for an LLM FM.

#### Data Processing and Flattening
**JSON Data**: The data from the S3 bucket was processed using the `jq` tool to extract relevant events. The goal was to create a database schema to evaluate player performance through features like damage dealt, players killed, and assists.

**PowerShell and Python Tools**: 
- **PowerShell Script**: Used to filter and summarize large VAL data sets.
- **Python Script**: Developed to process JSON files using user inputs and convert them to CSV for integration with AWS.

**Text Extraction**: Textual data was extracted from XML files using Beautiful Soup and other libraries to support the Amazon Kendra knowledge base.

#### AWS Architecture
Following the tutorial "Develop advanced generative AI chat-based assistants by using RAG and ReAct prompting," the project utilized various AWS services like Amazon S3, AWS Glue, and Amazon RDS. The solution, named Los Valientes, leverages Amazon Bedrock and Streamlit for team building and providing insights into Valorant Esports players, enhancing the user experience with an LLM-backed chat-based assistant.
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/VCT_HAck/arch.png" title="AWS Services" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/VCT_HAck/Lambda.png" title="Lambda Container" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
