# Conversational Bot using Hugging Face Transformers

## Project Overview

This project implements a basic conversational bot using Hugging Face's DialoGPT-medium model. The bot interacts with users to collect their name, email, and company information, validating email formats and storing the collected data in an in-memory dictionary.

## Objectives

- Design a user-friendly conversational flow for the bot.
- Utilize Hugging Face Transformers to enhance natural language understanding.
- Collect and store user data securely in-memory.

## Framework Choice

### Why Hugging Face Transformers?

- **Advanced NLP Capabilities**: Leverages state-of-the-art language models.
- **Ease of Integration**: Simplifies adding conversational AI to applications.
- **Support for Pre-trained Models**: Provides a variety of pre-trained models for different tasks.

## Implementation Details

### Model and Tokenizer Setup


from transformers import AutoModelForCausalLM, AutoTokenizer
import torch
import re

# Initialize the model and tokenizer
model_name = "microsoft/DialoGPT-medium"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(model_name)


### Response Generation Function


# Function to generate response from the bot
def generate_response(user_input, chat_history_ids):
    new_user_input_ids = tokenizer.encode(user_input + tokenizer.eos_token, return_tensors='pt')
    bot_input_ids = torch.cat([chat_history_ids, new_user_input_ids], dim=-1) if chat_history_ids is not None else new_user_input_ids
    chat_history_ids = model.generate(bot_input_ids, max_length=1000, pad_token_id=tokenizer.eos_token_id)
    bot_response = tokenizer.decode(chat_history_ids[:, bot_input_ids.shape[-1]:][0], skip_special_tokens=True)
    return bot_response, chat_history_ids


### User Input Handling


# In-memory storage for collected information
user_info = {}

# Start the conversation
print("Bot: Hello! Welcome to our service. May I know your name?")
user_name = input("You: ")
user_info['name'] = user_name

# Initialize chat history
chat_history_ids = None
bot_response, chat_history_ids = generate_response(f"My name is {user_name}", chat_history_ids)
print(f"Bot: Nice to meet you, {user_name}! Can I have your email?")

# Validate email
def validate_email(email):
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

# Ask for email with validation
while True:
    user_email = input("You: ")
    if validate_email(user_email):
        user_info['email'] = user_email
        break
    else:
        print("Bot: That doesn't seem like a valid email address. Could you please enter a valid email?")

bot_response, chat_history_ids = generate_response(f"My email is {user_email}", chat_history_ids)
print("Bot: Great! Lastly, could you tell me which company you are from?")

# Ask for company
user_company = input("You: ")
user_info['company'] = user_company
bot_response, chat_history_ids = generate_response(f"I am from {user_company}", chat_history_ids)

# Thank the user and display collected information
print(f"Bot: Thank you, {user_name}! Here is the information you provided:")
print(user_info)

# Enhanced response
print(f"Bot: It was a pleasure assisting you, {user_name}. If you have any more questions or need further assistance, feel free to ask!")


## Features and Demonstration

- **Natural Language Processing**: Utilizes Hugging Face Transformers for advanced conversational AI.
- **Email Validation**: Ensures user-provided emails are in the correct format.
- **Error Handling**: Provides prompts for invalid inputs to guide users.

### Demonstration

Include screenshots or a video showing the bot's interaction flow, from greeting to collecting user information and providing responses.

## Conclusion and Future Enhancements

### Summary

- Successfully implemented a conversational bot using Hugging Face Transformers.
- Validated email inputs and handled user interactions gracefully.

### Future Enhancements

- Improve memory handling and context retention.
- Expand user interaction features for a more dynamic experience.
