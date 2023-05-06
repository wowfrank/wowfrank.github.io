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



## Prompting Principles

1. Principle 1: Write clear and specific instructions
  - Tactic 1: Use delimiters to clearly indicate distinct parts of the input: ```, """, \< \>, \<tag\> \<\/tag\>, :
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

### Inferring

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
4. Doing multiple tasks at once
5. Inferring topics

### 以前的税没有退，现在过期了吗？

没有过期！可以往前申报5年的税务，但是需要用纸质表格并且邮寄申报。

![加拿大报税]({{site.baseurl}}/assets/img/2023-04-23/v2-3188f8da803b5cb7bbf5340248eb835b_720w.webp)

## 留学生报税可以得到的补贴或者是退税

- **货劳税/统一销售税补贴(GST/HST CREDIT)**  
  加拿大的移民、国际留学生，逗留半年以上的访问学者、探亲人士，都可申请该项补贴。值得注意的是这项补贴即使您有权得到，您仍须申请。如不申请，政府不会主动付给您这项补贴。所以留学生要想收到这项补贴，首先必须要完成报税，并且这一部分补贴是免税的。
- **住房补贴**  
  安省居民，也包括含国际留学生，如果支付了房租并且无收入或者低收入可以得到部分房租补贴。如果要申请该项补贴，须提供：居住地址、居住时间、交付的房租数额、房东姓名(或房产公司名称)。如果您在一年中居住过不同的地方，每个地方都要填写。需要提醒大家的是，尽管报税时是不需要提供房租收据的，但是申报这项补贴者一定要保留好房租收据，因为税务局有可能在退给申请者此项补贴后向申请者来信索要房租收据。如果申请者不能提供收据，税务局会要求申请者退回补贴并且加收利息。如果您在留学期间购买了房产，那么就您缴纳的地税(Property Tax)部分，根据您的收入情况，同样可以得到相应比例的住房补贴。
- **学费的申报 (TUITION FEECREDIT)**  
  这一块是留学生需要重点关注的地方，因为留学的费用累计起来往往是一笔很大的数目。如果您报了学费，以后一旦您有了收入而需要交税时，学费就会帮助您多退税。简单计算一下，如果您留学期间一共花了$70,000元，您毕业后年均收入40,000万元，则这项抵扣通常能节省您约15000元。还有加拿大税局有一个非常人性化的规定，即使您当年没有收入或收入不够，如果你已婚或有孩子，您还可以将您的学费的Credit Tansfer给您的配偶或孩子。

![加拿大报税]({{site.baseurl}}/assets/img/2023-04-23/v2-b1cafa4d617e2ffbc0fba7a4db97166a_720w.webp)

## 第一次报税所需材料

1. **社会保险号(Social Insurance Number)**  
   也就是俗称的工卡号所有post-secondary的学签都允许学生校外打工。学生需要去Service Canada 办理并取得工卡号。如果学生是第一次报税，又没有社会保险卡号，则需要向税务局申请Individual Tax Number(ITN),即临时税号，一个0开头的9位号码。
2. **T2202A或者T2202**  
   这个是学校给学生开具的一个用于学费报税的表格一般学生可以在学校的网站上下载,也可以直接找学生办公室索要或要求补开已丢失的收据。学费的报税属于不可退回的Credit,但是在学生毕业后有收入时才可以起到抵减收入的作用，从而达到少纳税的目的。
3. **T4，T4A，T5等收入证明**  
   T4：如果学生在校外工作，公司会在年初给学生开一张T4表。T4A：奖学金收入T5：银行利息收入，这是银行给学生邮寄的一张T5表格。只有学生的利息多于50元，银行才会给学生邮寄这个T5表格。所以不需要担心没有收到。无论什么表，只有带有“For income tax purposes”，都要妥善保存，报税时会用到。
4. **房租收据，地税(Property Tax)**  
   这是一个支付了房租并且无收入或者低收入可以得到部分房租补贴。如果要申请该项补贴，需提供：居住地址，居住时间，支付的房租数额，房东姓名(或房产公司名称)和联系方式。如果学生在一年中居住过不同的地方，每个地方都要填写。尽管报税时是不需要提供房租收据，但是一定要保留好房租收据，因为税务局有可能在退给申请者房租补贴后，向申请者来信索要房租收据。地税是针对留学期间购买了房产，需要提供你的地税单据，同样可以得到相应比例的住房补贴。
5. **邮寄地址或支票**  
   方便CRA给学生寄支票或者直接将退税金额转到学生的银行账户

![加拿大报税]({{site.baseurl}}/assets/img/2023-04-23/v2-ae7882cb06191fc508907ec270345845_720w.webp)

## 报税流程

1. 下载免费报税软件 [StudioTax](http://www.studiotax.com/en/)
1. 准备好SIN号、住址、学费单T2202A
1. 有收入的同学要准备T4、T4A、T3、T5以及奖学金等单据
1. 按照软件步骤填写就好
1. 打印出来寄到税务局或NETFLE在线递交即可。

### 打印的话注意：

- 把pdf打印出来检查一下姓名，地址，SIN号是否正确，在最后签上字和日期。
- 填上银行转账的信息，税务局会把钱打到你的账户，不填的话税务局会给你寄支票，然后和学费单2202A拷贝一起用信封寄到税务局。
- 税务局地址如下：
  International Tax Services Office
  Canada Revenue AgencyPost Office Box 9769, Station TOttawa ON K1G 3Y4CANADA

#### 源自[ChatGPT Prompt Engineering for Developers](https://learn.deeplearning.ai/chatgpt-prompt-eng/lesson/1/introduction)
