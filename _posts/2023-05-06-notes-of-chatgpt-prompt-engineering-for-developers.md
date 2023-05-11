---
layout: post
title: "Notes of ChatGPT Prompt Engineering for Developers"
date: "2023-05-06 01:09:00 +0800"
description: "ChatGPT Prompt Engineering for Developers" # (optional)
img: "2023-05-06-cover-image-notes-of-chatgpt-prompt-engineering-for-developers.jpg" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['ChatGPT', 'Prompt', 'AI']
categories: ['ChatGPT', 'AI']
---

## Setup

```python
import os
import openai
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file

openai.api_key  = os.getenv('OPENAI_API_KEY')

def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]

def get_completion_from_messages(messages, model="gpt-3.5-turbo", temperature=0):
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature, # this is the degree of randomness of the model's output
    )
#     print(str(response.choices[0].message))
    return response.choices[0].message["content"]

messages =  [  
{'role':'system', 'content':'You are an assistant that speaks like Shakespeare.'},    
{'role':'user', 'content':'tell me a joke'},   
{'role':'assistant', 'content':'Why did the chicken cross the road'},   
{'role':'user', 'content':'I don\'t know'}  ]

response = get_completion_from_messages(messages, temperature=1)
print(response)
```

## Prompting Principles

1. Principle 1: Write clear and specific instructions
  - Tactic 1: Use delimiters to clearly indicate distinct parts of the input: ```, """, \< \>, \<tag\> \</tag\>, :
    - eg:
      ```python
      prompt = f"""
      Summarize the text delimited by triple backticks \ 
      into a single sentence.
      ```{text}```
      """
      ```

  - Ask for a structured output: JSON, HTML
    - eg:
      ```python
      prompt = f"""
      Generate a list of three made-up book titles along \ 
      with their authors and genres. 
      Provide them in JSON format with the following keys: 
      book_id, title, author, genre.
      """
      ```

  - Ask the model to check whether conditions are satisfied
    - eg:
      ```python
      prompt = f"""
      You will be provided with text delimited by triple quotes. 
      If it contains a sequence of instructions, \ 
      re-write those instructions in the following format:

      Step 1 - ...
      Step 2 - …
      …
      Step N - …

      If the text does not contain a sequence of instructions, \ 
      then simply write \"No steps provided.\"

      \"\"\"{text_1}\"\"\"
      """
      ```

  - "Few-shot" prompting
    - eg:
      ```python
      prompt = f"""
      Your task is to answer in a consistent style.

      <child>: Teach me about patience.

      <grandparent>: The river that carves the deepest \ 
      valley flows from a modest spring; the \ 
      grandest symphony originates from a single note; \ 
      the most intricate tapestry begins with a solitary thread.

      <child>: Teach me about resilience.
      """
      ```
      
2. Principle 2: Give the model time to “think”
  - Specify the steps required to complete a task
    - eg:
      ```python
      prompt_2 = f"""
      Your task is to perform the following actions: 
      1 - Summarize the following text delimited by 
      <> with 1 sentence.
      2 - Translate the summary into French.
      3 - List each name in the French summary.
      4 - Output a json object that contains the 
      following keys: french_summary, num_names.

      Use the following format:
      Text: <text to summarize>
      Summary: <summary>
      Translation: <summary translation>
      Names: <list of names in Italian summary>
      Output JSON: <json with summary and num_names>

      Text: <{text}>
      """
      ```

  - Instruct the model to work out its own solution before rushing to a conclusion
      
      ```python
      prompt = f"""
      Your task is to determine if the student's solution \
      is correct or not.
      To solve the problem do the following:
      - First, work out your own solution to the problem. 
      - Then compare your solution to the student's solution \ 
      and evaluate if the student's solution is correct or not. 
      Don't decide if the student's solution is correct until 
      you have done the problem yourself.

      Use the following format:
      Question:
      \"\"\"
      question here
      \"\"\"
      Student's solution:
      \"\"\"
      student's solution here
      \"\"\"
      Actual solution:
      \"\"\"
      steps to work out the solution and your solution here
      \"\"\"
      Is the student's solution the same as actual solution \
      just calculated:
      \"\"\"
      yes or no
      \"\"\"
      Student grade:
      \"\"\"
      correct or incorrect
      \"\"\"

      Question:
      \"\"\"
      I'm building a solar power installation and I need help \
      working out the financials. 
      - Land costs $100 / square foot
      - I can buy solar panels for $250 / square foot
      - I negotiated a contract for maintenance that will cost \
      me a flat $100k per year, and an additional $10 / square \
      foot
      What is the total cost for the first year of operations \
      as a function of the number of square feet.
      \"\"\" 
      Student's solution:
      \"\"\"
      Let x be the size of the installation in square feet.
      Costs:
      1. Land cost: 100x
      2. Solar panel cost: 250x
      3. Maintenance cost: 100,000 + 100x
      Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000
      \"\"\"
      Actual solution:
      """
      ```

## Iterative Prompt Develelopment

1. Limit the number of words/sentences/characters
   1. eg: "Use at most 50 words"
2. Text focuses on the wrong details
   1. Ask it to focus on the aspects that are relevant to the intended audience
      1. eg: "At the end of the description, include every 7-character Product ID in the technical specification"
3. Description needs a table of dimensions
   1. Ask it to extract information and organize it in a table
      1. eg: "Give the table the title 'Product Dimensions'. Format everything as HTML that can be used in a website. Place the description in a \<div\> element."

## Summarizing

1. Summarize with a word/sentence/character limit
   1. eg:
      ```python
      prompt = f"""
      Your task is to generate a short summary of a product \
      review from an ecommerce site. 

      Summarize the review below, delimited by triple 
      backticks, in at most 30 words. 

      Review: ```{prod_review}```
      """
      ```

2. Summarize with a focus on shipping and delivery
   1. eg:
      ```python
      prompt = f"""
      Your task is to generate a short summary of a product \
      review from an ecommerce site to give feedback to the \
      Shipping deparmtment. 

      Summarize the review below, delimited by triple 
      backticks, in at most 30 words, and focusing on any aspects \
      that mention shipping and delivery of the product. 

      Review: ```{prod_review}```
      """
      ```

3. Summarize with a focus on price and value
   1. eg:
      ```python
      prompt = f"""
      Your task is to generate a short summary of a product \
      review from an ecommerce site to give feedback to the \
      pricing deparmtment, responsible for determining the \
      price of the product.  

      Summarize the review below, delimited by triple 
      backticks, in at most 30 words, and focusing on any aspects \
      that are relevant to the price and perceived value. 

      Review: ```{prod_review}```
      """
      ```

4. Try "extract" instead of "summarize"
   1. eg:
      ```python
      prompt = f"""
      Your task is to extract relevant information from \ 
      a product review from an ecommerce site to give \
      feedback to the Shipping department. 

      From the review below, delimited by triple quotes \
      extract the information relevant to shipping and \ 
      delivery. Limit to 30 words. 

      Review: ```{prod_review}```
      """
      ```

5. Summarize multiple product reviews
   1. eg:
   ```python
   ```

## Inferring

1. Sentiment (positive/negative)
   1. eg:
      ```python
      prompt = f"""
      What is the sentiment of the following product review, 
      which is delimited with triple backticks?

      Give your answer as a single word, either "positive" or "negative".

      Review text: '''{lamp_review}'''
      """
      ```

2. Identify types of emotions
   1. Identify anger, eg:
      ```python
      prompt = f"""
      Is the writer of the following review expressing anger? The review is delimited with triple backticks. Give your answer as either yes or no.

      Review text: '''{lamp_review}'''
      """
      ```

3. Extract product and company name from customer reviews
   1. Doing multiple tasks at once, eg:
      ```python
      prompt = f"""
      Identify the following items from the review text: 
      - Sentiment (positive or negative)
      - Is the reviewer expressing anger? (true or false)
      - Item purchased by reviewer
      - Company that made the item

      The review is delimited with triple backticks. \
      Format your response as a JSON object with \
      "Sentiment", "Anger", "Item" and "Brand" as the keys.
      If the information isn't present, use "unknown" \
      as the value.
      Make your response as short as possible.
      Format the Anger value as a boolean.

      Review text: '''{lamp_review}'''
      """
      ```

4. Inferring 5 topics
   1. eg:
      ```python
      prompt = f"""
      Determine five topics that are being discussed in the \
      following text, which is delimited by triple backticks.

      Make each item one or two words long. 

      Format your response as a list of items separated by commas.

      Text sample: '''{story}'''
      """
      ```

## Transforming

1. Translation
   1. eg:
      ```python
      prompt = f"""
      Translate the following English text to Spanish: \ 
      ```Hi, I would like to order a blender```
      """
      ```

   2. eg:
      ```python
      prompt = f"""
      Tell me which language this is: 
      ```Combien coûte le lampadaire?```
      """
      ``` 

   3. eg:
      ```python
      prompt = f"""
      Translate the following text to Spanish and French in both the \
      formal and informal forms: 
      'Would you like to order a pillow?'
      """
      ```

2. Universal Translator
   1. eg:
      ```python
      user_messages = [
         "La performance du système est plus lente que d'habitude.",  # System performance is slower than normal         
         "Mi monitor tiene píxeles que no se iluminan.",              # My monitor has pixels that are not lighting
         "Il mio mouse non funziona",                                 # My mouse is not working
         "Mój klawisz Ctrl jest zepsuty",                             # My keyboard has a broken control key
         "我的屏幕在闪烁"                                               # My screen is flashing
      ] 
      for issue in user_messages:
         prompt = f"Tell me what language this is: ```{issue}```"
         lang = get_completion(prompt)
         print(f"Original message ({lang}): {issue}")

         prompt = f"""
         Translate the following  text to English \
         and Korean: ```{issue}```
         """
         response = get_completion(prompt)
         print(response, "\n")
      ```

3. Tone Transformation
   1. eg:
      ```python
      prompt = f"""
      Translate the following from slang to a business letter: 
      'Dude, This is Joe, check out this spec on this standing lamp.'
      """
      ```

4. Format Conversion
   1. eg:
      ```python
      data_json = { "resturant employees" :[ 
         {"name":"Shyam", "email":"shyamjaiswal@gmail.com"},
         {"name":"Bob", "email":"bob32@gmail.com"},
         {"name":"Jai", "email":"jai87@gmail.com"}
      ]}

      prompt = f"""
      Translate the following python dictionary from JSON to an HTML \
      table with column headers and title: {data_json}
      """
      ```

5. Spellcheck/Grammar check
   1. eg:
      ```python
      text = [ 
      "The girl with the black and white puppies have a ball.",  # The girl has a ball.
      "Yolanda has her notebook.", # ok
      "Its going to be a long day. Does the car need it’s oil changed?",  # Homonyms
      "Their goes my freedom. There going to bring they’re suitcases.",  # Homonyms
      "Your going to need you’re notebook.",  # Homonyms
      "That medicine effects my ability to sleep. Have you heard of the butterfly affect?", # Homonyms
      "This phrase is to cherck chatGPT for speling abilitty"  # spelling
      ]
      for t in text:
         prompt = f"""Proofread and correct the following text
         and rewrite the corrected version. If you don't find
         and errors, just say "No errors found". Don't use 
         any punctuation around the text:
         ```{t}```"""
         response = get_completion(prompt)
         print(response)
      ```

## Expanding

1. Customize the automated reply to a customer email
   1. eg: 
      ```python
      prompt = f"""
      You are a customer service AI assistant.
      Your task is to send an email reply to a valued customer.
      Given the customer email delimited by ```, \
      Generate a reply to thank the customer for their review.
      If the sentiment is positive or neutral, thank them for \
      their review.
      If the sentiment is negative, apologize and suggest that \
      they can reach out to customer service. 
      Make sure to use specific details from the review.
      Write in a concise and professional tone.
      Sign the email as `AI customer agent`.
      Customer review: ```{review}```
      Review sentiment: {sentiment}
      """
      ```

2. Remind the model to use details from the customer's email
   1. eg:
      ```python
      prompt = f"""
      You are a customer service AI assistant.
      Your task is to send an email reply to a valued customer.
      Given the customer email delimited by ```, \
      Generate a reply to thank the customer for their review.
      If the sentiment is positive or neutral, thank them for \
      their review.
      If the sentiment is negative, apologize and suggest that \
      they can reach out to customer service. 
      Make sure to use specific details from the review.
      Write in a concise and professional tone.
      Sign the email as `AI customer agent`.
      Customer review: ```{review}```
      Review sentiment: {sentiment}
      """
      ```

## Chatbot

1. OrderBot
   1. eg:
      ```python
      import panel as pn  # GUI
      pn.extension()

      panels = [] # collect display 

      context = [ {'role':'system', 'content':"""
      You are OrderBot, an automated service to collect orders for a pizza restaurant. \
      You first greet the customer, then collects the order, \
      and then asks if it's a pickup or delivery. \
      You wait to collect the entire order, then summarize it and check for a final \
      time if the customer wants to add anything else. \
      If it's a delivery, you ask for an address. \
      Finally you collect the payment.\
      Make sure to clarify all options, extras and sizes to uniquely \
      identify the item from the menu.\
      You respond in a short, very conversational friendly style. \
      The menu includes \
      pepperoni pizza  12.95, 10.00, 7.00 \
      cheese pizza   10.95, 9.25, 6.50 \
      eggplant pizza   11.95, 9.75, 6.75 \
      fries 4.50, 3.50 \
      greek salad 7.25 \
      Toppings: \
      extra cheese 2.00, \
      mushrooms 1.50 \
      sausage 3.00 \
      canadian bacon 3.50 \
      AI sauce 1.50 \
      peppers 1.00 \
      Drinks: \
      coke 3.00, 2.00, 1.00 \
      sprite 3.00, 2.00, 1.00 \
      bottled water 5.00 \
      """} ]  # accumulate messages

      inp = pn.widgets.TextInput(value="Hi", placeholder='Enter text here…')
      button_conversation = pn.widgets.Button(name="Chat!")

      interactive_conversation = pn.bind(collect_messages, button_conversation)

      dashboard = pn.Column(
         inp,
         pn.Row(button_conversation),
         pn.panel(interactive_conversation, loading_indicator=True, height=300),
      )

      dashboard

      messages =  context.copy()
      messages.append(
      {'role':'system', 'content':'create a json summary of the previous food order. Itemize the price for each item\
      The fields should be 1) pizza, include size 2) list of toppings 3) list of drinks, include size   4) list of sides include size  5)total price '},    
      )
      #The fields should be 1) pizza, price 2) list of toppings 3) list of drinks, include size include price  4) list of sides include size include price, 5)total price '},    

      response = get_completion_from_messages(messages, temperature=0)
      print(response)

      def collect_messages(_):
         prompt = inp.value_input
         inp.value = ''
         context.append({'role':'user', 'content':f"{prompt}"})
         response = get_completion_from_messages(context) 
         context.append({'role':'assistant', 'content':f"{response}"})
         panels.append(
            pn.Row('User:', pn.pane.Markdown(prompt, width=600)))
         panels.append(
            pn.Row('Assistant:', pn.pane.Markdown(response, width=600, style={'background-color': '#F6F6F6'})))
      
         return pn.Column(*panels)
      ```

#### 源自[ChatGPT Prompt Engineering for Developers](https://learn.deeplearning.ai/chatgpt-prompt-eng/lesson/1/introduction)
