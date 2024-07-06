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


![image](https://github.com/tarun261003/WebMobi/assets/122869742/31a337c0-2a69-47a2-a1cc-ac66e2784d24)


# Initialize the model and tokenizer
![image](https://github.com/tarun261003/WebMobi/assets/122869742/d89c23d4-51d7-4fe7-be4e-abd87a64244b)



### Response Generation Function


# Function to generate response from the bot
![image](https://github.com/tarun261003/WebMobi/assets/122869742/26f9162a-f117-4032-8a8e-e905b0e5dff2)




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
![image](https://github.com/tarun261003/WebMobi/assets/122869742/d0070109-0501-439c-b943-156f4db72d24)


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
